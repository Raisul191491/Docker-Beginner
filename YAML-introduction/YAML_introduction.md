এই পর্যায়ে আমরা YAML ফাইলের সাথে পরিচিত হবো...
---
```YAML``` সম্পর্কে জানা অনেক গুরুত্বপূর্ণ কেননা আমাদের শেখার পরবর্তী ধাপগুলোতে এর ব্যাপক ব্যবহার আমরা লক্ষ্য করব। যদি কারো ```XML``` বা ```JSON``` এর সাথে আগে থেকে পরিচয় থেকে থাকে তাহলে ```YAML``` বোঝাটা খুব সহজ হয়ে যাবে।

```YAML``` ফাইলের ব্যবহার হয় ডাটা রিপ্রেজেন্ট করার জন্য, এক্ষেত্রে কনফিগারেশন ডাটা। নিচেই উদাহরণ দেয়া হলো ৩ ধরনের ডাটা ফরম্যাটের-

![yaml](/YAML-introduction/yaml.png)

সর্ববামে আমরা দেখছি ```XML```, যেখানে আমরা একটা সার্ভার লিস্ট এবং সম্পর্কিত তথ্য দেখতে পারছি। একই তথ্যগুলো ```JSON``` ফরম্যাটে মাঝে এবং সর্বডানে ```YAML``` এ রিপ্রেজেন্ট হয়েছে।

এবারে YAML এর আরেকটু গভীরে প্রবেশ করা যাক...
---
যদি আমরা যেকোনো ডাটা কে একদম সাধারণ ফরম্যাটে কল্পনা করি তাহলে তা হচ্ছে **[ ```Key``` , ```Value``` ]** পেয়ার হিসেবে। যেমন, Apple, Key এর Value হতে পারে Fruit. এটাকে YAML এ প্রকাশ করার পদ্ধতি টা নিম্নরূপ-

![key](/YAML-introduction/key.png)

এখানে ```Key``` এবং ```Value``` একটি কোলন ( : ) দ্বারা আলাদা করা আছে। এখানে কোলনের পরে একটা স্পেস দেয়া বাধ্যতামূলক। 

এবারে দেখি ```YAML``` এ কিভাবে একটি অ্যারে প্রকাশ করা হয়-

![array](/YAML-introduction/array.png)

এখানে অ্যারে নামের পরে একটা কোলন দিয়ে এর এলিমেন্ট গুলো পরের লাইন থেকে আগে একটি করে ড্যাশ দিয়ে লিখতে হবে। 

ডিকশিনারি বা ম্যাপের ক্ষেত্রে রিপ্রেজেন্ট টা হবে নিম্নরূপ -

![dic](/YAML-introduction/dic.png)

এখানে একটি আইটেমের যতগুলো প্রোপার্টি রয়েছে সবগুলোর আগে স্পেস থাকতে হবে এবং সমান সংখ্যক স্পেস হতে হবে সেটা। এই স্পেস এর ব্যাপারটা YAML এর ক্ষেত্রে অনেক বেশি সেনসিটিভ।  যদি উপরের ছবিটায় Calories এর নিচের ২ টা প্রোপার্টির আগে একটা করে বেশি স্পেস থাকত তাহলে সে ২ টা Calories এর প্রোপার্টি হয়ে যেত এবং একটা সিনট্যাক্স এরোর আসত কেননা Calories এর ভ্যালু আগে থেকেই দিয়ে দিয়েছি। 

![spaces](/YAML-introduction/spaces.png)

এগুলো মিক্স ম্যাচ করে আমরা অনেক ধরনের রিপ্রেজেন্টেশন পেতে পারি। যেমন, একটা লিস্টের ভেতর আরেকটা ডিকশনারি এবং তার ভেতরে আরো একটা লিস্ট-

![mix](/YAML-introduction/mix.png)

একটি অবজেক্টের অনেকগুলো বৈশিষ্ট্য থাকলে আমরা তা রিপ্রেজেন্ট করি ডিকশিনারি দিয়ে-

![dict](/YAML-introduction/dic.png)

একই ধরনের অনেকগুলো অবজেক্ট এর আইডি বা নাম স্টোর করতে ব্যবহৃত হয় লিস্ট বা অ্যারে-

![cars](/YAML-introduction/cars.png)

এবারে যদি আমরা প্রত্যেক অবজেক্টের নামের সাথে তাদের বৈশিষ্ট্য গুলোও যোগ করে রাখতে চাই তাহলে অবজেক্টের লিস্টের প্রত্যেক এলিমেন্টে একটা করে ডিকশিনারি রাখব বা ডিকশনারি লিস্ট ব্যবহার করব-

![list dic](/YAML-introduction/list%20dic.png) 

ডিকশনারি হচ্ছে **Unordered collection** এবং লিস্ট হচ্ছে **Ordered collection**. ডিকশিনারির ক্ষেত্রে যদি ২ টি একই নামের ডিকশনারিতে একই প্রোপার্টি গুলো থেকে থাকে তাহলেই তারা ইকুয়াল ধরে নিতে হবে। এক্ষেত্রে প্রোপার্টিগুলো কোন অর্ডারে লেখা আছে সেটাতে যায় আসে না।

লিস্ট বা অ্যারের ক্ষেত্রে ঠিক উলটো টা হয়। অর্ডার ঠিক না থাকলে একই এলিমেন্ট থাকা সত্ত্বেও ২ টি লিস্ট ইকুয়াল হবে না। ডাটা স্ট্রাকচার নিয়ে কাজ করার সময় এই বিষয়গুলি মাথায় রাখতে হবে।

YAML এ কমেন্ট লেখার সিস্টেম হচ্ছে লেখার আগে একটা # যুক্ত করে দেয়া

![comment](/YAML-introduction/comment.png)


[previous page](https://github.com/Raisul191491/Docker-Beginner/blob/main/Docker-advance-introduction/listing.md) <--> [next page](https://github.com/Raisul191491/Docker-Beginner/blob/main/Conclusion/Conclusion.md)
