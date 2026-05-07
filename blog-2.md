# Keep Your Code DRY: How `Pick` and `Omit` Eliminate Interface Duplication

## Introduction

One of the most common sources of duplication in TypeScript codebases is defining multiple interfaces that share most of their properties. You start with a master `User` interface, then create a `UserPreview` with fewer fields, then a `UserFormData` with slightly different ones — and suddenly you're maintaining three nearly identical types. Change one property name and you have to hunt down every duplicate.

TypeScript's built-in `Pick` and `Omit` utility types solve this elegantly by letting you derive specialized "slices" of a master interface without repeating yourself.

---

## The Problem: Manual Duplication

Imagine a master interface for a user in your system:

```typescript
interface User {
  id: number;
  name: string;
  email: string;
  passwordHash: string;
  createdAt: Date;
  role: "admin" | "user" | "guest";
}
```

Now you need:
- A `UserPreview` for list views (just `id`, `name`, `role`)
- A `UserFormData` for a registration form (everything *except* `id`, `passwordHash`, and `createdAt`)

Without utility types, you'd duplicate:

```typescript
// ❌ Repeated, error-prone, hard to maintain
interface UserPreview {
  id: number;
  name: string;
  role: "admin" | "user" | "guest";
}

interface UserFormData {
  name: string;
  email: string;
  role: "admin" | "user" | "guest";
}
```

If you rename `role` to `userRole` in `User`, you now have three places to update instead of one.

---

## `Pick`: Select Only What You Need

`Pick<T, K>` constructs a new type by selecting a subset of keys `K` from type `T`.

```typescript
type UserPreview = Pick<User, "id" | "name" | "role">;

// Equivalent to:
// { id: number; name: string; role: "admin" | "user" | "guest" }
```

Now `UserPreview` is derived directly from `User`. Rename a property in `User` and TypeScript immediately flags every `Pick` that referenced the old name — no silent mismatches.

```typescript
function renderUserCard(user: UserPreview): string {
  return `[${user.role.toUpperCase()}] ${user.name} (ID: ${user.id})`;
}
```

---

## `Omit`: Exclude What You Don't Want

`Omit<T, K>` is the inverse — it constructs a type with all properties of `T` *except* those listed in `K`.

```typescript
type UserFormData = Omit<User, "id" | "passwordHash" | "createdAt">;

// Equivalent to:
// { name: string; email: string; role: "admin" | "user" | "guest" }
```

This is ideal when you want most of an interface but need to strip out server-generated or sensitive fields.

```typescript
function registerUser(formData: UserFormData): void {
  // id, passwordHash, createdAt are not available here — by design
  console.log(`Registering ${formData.name} as ${formData.role}`);
}
```

---

## Combining with Other Utility Types

`Pick` and `Omit` become even more powerful when composed with other utilities:

```typescript
// A public-facing user profile: no sensitive fields, all optional for partial updates
type PublicUserProfile = Partial<Omit<User, "passwordHash">>;

// A strictly-typed update payload for admins
type AdminUserUpdate = Pick<User, "role" | "email"> & { updatedBy: number };
```

---

## The DRY Payoff

| Without Utility Types | With `Pick` / `Omit` |
|---|---|
| Multiple copies of overlapping properties | One source of truth (`User`) |
| Refactoring requires changes in many files | Change `User`, derived types update automatically |
| Easy to miss a field when copying | Structural relationship is explicit in code |
| Reviewers must compare interfaces manually | Intent is self-documenting |

---

## Conclusion

`Pick` and `Omit` are TypeScript's way of enforcing the DRY principle at the type level. Instead of copy-pasting interface definitions and maintaining them in parallel, you express the *relationship* between types — "this is a subset of User" or "this is User without the sensitive parts." Your code becomes easier to refactor, harder to break, and clearer to read. Whenever you catch yourself writing an interface that looks suspiciously like another one with a few fields added or removed, reach for `Pick` or `Omit` instead.
