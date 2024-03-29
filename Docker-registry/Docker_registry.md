# Docker Registry

এই লেকচারে আমরা ডকার রেজিস্ট্রি সম্বন্ধে জানবো। 

## ডকার রেজিস্ট্রি কী? 

ডকার রেজিস্ট্রি হলো ডকার ইমেজগুলোর জন্যে একটি সংরক্ষণাগার এবং বিতরণ মাধ্যম। যদি কনটেইনারগুলো-কে আমরা `বৃষ্টি` মনে করি তাহলে ডকার রেজিস্ট্রি হলো `মেঘ`। ডকার রেজিস্ট্রি সব ডকার ইমেজের মূল সংগ্রহস্থল। 

![](Screenshot%20from%202022-04-30%2016-29-53.png)

## আমরা একটি `nginx` কনটেইনারের উদাহরণ দিয়ে বিষয়টা বোঝার চেষ্টা করি - 

- মনে করো তুমি `docker run nginx` কম্যান্ড ব্যবহার করে `nginx` ইমেজের একটি কনটেইনার রান করলে। এখন এই `nginx` জিনিসটা কী? কোথায় থেকে এটি আসে? 

![](Screenshot%20from%202022-04-30%2016-20-53.png)

  `nginx` নামটি ডকার ইমেজ নেমিং কনভেনশন অনুসরণ করে। এখানে nginx হলো একটি ইমেজ বা রিপোজিটরি নাম। যখন আমরা বলি `nginx`, আসলে সেটা `nginx/nginx`। প্রথম অংশ ইউজার বা অ্যাকাউন্ট নির্দেশ করে। যদি কোন অ্যাকাউন্ট বা রিপোজিটরি উল্লেখ করে দেওয়া না হয় তাহলে এটি যেই নাম দেওয়া আছে সেটাই ধরে নেই ( এক্ষেত্রে সেটা nginx)। ইউজার নেম আসলে ডকার হাব অ্যাকাউন্ট নেম অথবা একটি সংগঠনের নাম। যখন তুমি নিজের একটি অ্যাকাউন্ট তৈরি করবে এবং এতে নিজের ইচ্ছামতো রিপোজিটরি বা ইমেজ বানিয়ে রাখবে তখন এইভাবেই তৈরি করতে হবে। 

![](Screenshot%20from%202022-04-30%2016-21-44.png)

![](Screenshot%20from%202022-04-30%2016-22-20.png)

- এখন এই ইমেজগুলো কোথায় জমা হয় এবং কোথায় থেকে পুল হয়? 
যেহেতু আমরা নির্দিষ্ট কোন স্থান বলে দিই নি যে কোথায় থেকে পুল করবে, তাই এটি ডকারের ডিফল্ট রেজিস্ট্রি ডকার হাব থেকে পুল করতে হবে ধরে নেয় যার DNS নাম docker.io (এই সেই রেজিস্ট্রি যেখানে সব ইমেজ সংরক্ষিত আছে)। যখন তুমি একটা নতুন ইমেজ তৈরি করো বা কোন ইমেজ আপডেট করো তখন সেটি এই রেজিস্ট্রিতে পুশ করতে হবে এবং যখন কেউ এই অ্যাপলিকেশন ডিপ্লই করবে তখন প্রত্যেকবারই এই রেজিস্ট্রি থেকে পুল হবে। 

![](Screenshot%20from%202022-04-30%2016-23-29.png)

![](Screenshot%20from%202022-04-30%2016-22-54.png)

ডকার হাব বাদেও আরও অনেক জনপ্রিয় রেজিস্ট্রি আছে। যেমন : `Google's Registry - gcr.io` যেখানে kubernetes সম্পর্কিত অনেক ইমেজ সংরক্ষিত আছে, `github's Registry - ghcr.io`

এগুলো সবই পাবলিক ইমেজ যেগুলো সবাই ব্যবহার  করতে পারে। কিন্তু যখন তোমার কোন অ্যাপলিকেশন যেটা পাবলিক রাখা ঠিক না বলে মনে করছো সেটা চাইলে তুমি প্রাইভেট করে দিতে পারো। 
অনেক cloud service provider (যেমন: AWS, AZURE বা DCP) by default প্রাইভেট রেজিস্ট্রি প্রদান করে যখন তুমি সেখানে একটি অ্যাকাউন্ট তৈরি করো। 

একটি প্রাইভেট রেজিস্ট্রি থেকে কোন ইমেজের কনটেইনার রান করতে গেলে প্রথমে `docker login` কম্যান্ড ব্যবহার করে প্রাইভেট রেজিস্ট্রিতে লগইন করতে হবে এবং প্রয়োজনীয় তথ্য প্রদান করতে হবে। তথ্য সঠিক হলে ইমেজ নাম হিসেবে নিম্নের মতো করে প্রাইভেট রেজিস্ট্রি ব্যবহার করতে হবে। 

![](Screenshot%20from%202022-04-30%2016-23-43.png)

এখন যদি তুমি প্রাইভেট রেজিস্ট্রিতে লগইন না করে `docker run` কম্যান্ড ব্যবহার করো তাহলে ইরর দেখাবে (ইমেজ নট ফাউন্ড)। 
সুতরাং পুল বা পুশ করার আগে অবশ্যই প্রাইভেট রেজিস্ট্রিতে লগইন করতে হবে। 

আমরা আগেই বলেছি যে, অনেক cloud service provider (যেমন: AWS, AZURE বা DCP) by default প্রাইভেট রেজিস্ট্রি প্রদান করে যখন তুমি সেখানে একটি অ্যাকাউন্ট তৈরি করো। কিন্তু যদি তুমি কোন অ্যাপলিকেশন premise-এ রান করো এবং কোন প্রাইভেট রেজিস্ট্রি না থাকে? তাহলে কীভাবে তোমার নিজের প্রাইভেট রেজিস্ট্রি সংগঠনে ডিপ্লয় করবে? 

- ডকার রেজিস্ট্রি নিজেই একটা অ্যাপলিকেশন এবং এটি ইমেজ হিসেবে অ্যাভেইলেবল আছে। ইমেজের নাম `registry` এবং পোর্ট `5000`-এ এটি এর API প্রকাশ করে। 

![](Screenshot%20from%202022-04-30%2016-48-34.png)

এখন তোমার কাস্টম রেজিস্ট্রি ডকার হোস্টের পোর্ট 5000-এ রান করছে, কীভাবে তুমি এখানে তোমার নিজের ইমেজ পুশ করবে? 

- এক্ষেত্রে `docker image tag` কম্যান্ড ব্যবহার করে ইমেজের tag-এ প্রাইভেট রেজিস্ট্রি অন্তর্ভূক্ত করতে হবে। এখানে যেহেতু কনটেইনারটি একই ডকার হোস্টে রান করছে আমরা `localhost:5000/image-name` ব্যবহার করতে পারি।  

![](Screenshot%20from%202022-04-30%2016-49-01.png)

ইমেজটি পুশ করতে `docker push` কম্যান্ড ব্যবহার করতে হবে। 

![](Screenshot%20from%202022-04-30%2016-51-37.png)

- ইমেজটি পুল করার জন্যে আমরা `localhost:5000` ব্যবহার করতে পারি (যদি একই হোস্টে হয়)। 

![](Screenshot%20from%202022-04-30%2016-51-54.png)

- অন্য হোস্ট থেকে পুল করতে হলে ঐ ডকার হোস্টের আইপি বা ডোমেইন ব্যবহার করে পুল করতে হবে।

![](Screenshot%20from%202022-04-30%2016-52-08.png)

[previous page](https://github.com/Raisul191491/Docker-Beginner/blob/main/Docker-networking/Docker_networking.md) <--> [next page](https://github.com/Raisul191491/Docker-Beginner/blob/main/Docker-on-Mac-and-Windows/docker_on_mac.md)
