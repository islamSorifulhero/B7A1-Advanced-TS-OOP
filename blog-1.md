Blog 1 (any vs unknown + type narrowing)
Title: Why any is Dangerous and unknown is Safer in TypeScript
Introduction

TypeScript-এর সবচেয়ে বড় শক্তি হচ্ছে type safety। কিন্তু any ব্যবহার করলে এই safety পুরোপুরি নষ্ট হয়ে যায়। এজন্য any কে বলা হয় "type safety hole"।

Why any is Dangerous
let value: any = "Hello";
value.toUpperCase(); // OK
value = 10;
value.toUpperCase(); // Runtime error

এখানে TypeScript কোনো error দেয় না, কারণ any সবকিছু allow করে।

Why unknown is Safer
let value: unknown = "Hello";

if (typeof value === "string") {
  console.log(value.toUpperCase());
}

এখানে TypeScript check করতে বাধ্য করে—এটাই safe।

Type Narrowing

Type narrowing মানে হচ্ছে type check করে specific type বের করা।

function printValue(value: unknown) {
  if (typeof value === "string") {
    return value.toUpperCase();
  }
  return "Not a string";
}
Conclusion

any ব্যবহার করলে bug detect করা কঠিন হয়। unknown ব্যবহার করলে code safe এবং predictable হয়।