এই লেকচারে আমরা কনটেইনার অর্কেস্ট্রেশন কী তা বোঝার চেষ্টা করবো। 

আমরা এতক্ষণ যাবৎ দেখে এসেছি `docker run` কম্যান্ড ব্যবহার করে যেকোন অ্যাপলিকেশনের একটি সিঙ্গেল ইন্সট্যান্স রান করা যায়। এখানে nodejs বেসড একটি অ্যাপলিকেশন রান করার জন্য `docker run nodejs` কম্যান্ড ব্যবহার করা হয়েছে। এটি ডকার হোস্টে অ্যাপলিকেশনটির শুধুমাত্র একটি ইন্সট্যান্স রান করে। 

![](Screenshot%20from%202022-05-15%2016-48-27.png)

কিন্তু যখন ইউজার সংখ্যা বেড়ে যাবে এবং ঐ ইন্সট্যান্স সেই লোড সামলাতে পারবে না তখন কী হবে? তখন চাইলে আমরা docker run কম্যান্ড ব্যবহার করে আরও অনেকগুলো ইন্সট্যান্স রান করতে পারি। 

![](Screenshot%20from%202022-05-15%2016-46-27.png)

কিন্তু এটি করতে হবে সম্পূর্ণ নিজ দায়িত্বে। আমাদের অ্যাপলিকেশনটির লোড ও পারফরম্যান্সের দিকে সবসময় খেয়াল রাখতে হবে এবং প্রয়োজনে নতুন ইন্সট্যান্স রান করতে হবে। যদি কোন কনটেইনার ফেইল করে তাহলে তা চিহ্নিত করতে হবে এবং পুনরায় তা রান করতে হবে। 

![](Screenshot%20from%202022-05-15%2016-39-54.png)
![](Screenshot%20from%202022-05-15%2016-40-35.png)


এক্ষেত্রে ডকার হোস্টের কথাও চিন্তা করতে হবে। যদি ডকার হোস্ট ক্র্যাশ করে এবং অ্যাক্সেসিবল না হয় তাহলে কী হবে? ঐ হোস্টে যেসব কনটেইনার ছিল সেগুলোও ইনঅ্যাক্সেসিবল হয়ে যাবে। 

![](Screenshot%20from%202022-05-15%2016-51-46.png)

সুতরাং এই পরিস্থিতি থেকে উত্তরণের জন্যে আমরা কী করতে পারি? আমাদের একটি নিবেদিত প্রকৌশলী দরকার যে এসব কনটেইনারগুলোর হেলথ ও পারফরম্যান্সে নজর রাখবে এবং কোন সমস্যার সৃষ্টি হলে তা সমাধানের ব্যবস্থা করবে। কিন্তু যখন হাজার হাজার কনটেইনারের একটি অ্যাপলিকেশন ডিপ্লয় হবে তখন এই পদ্ধতিটা ফলপ্রসু হবে না। তাই এই সমস্যা সমাধানের জন্য আমরা একটা script তৈরি করতে পারি। কনটেইনার অর্কেস্ট্রেশন হচ্ছে এই ধরনের একটি সলিউশ্যান। এটি অনেকগুলো টুলস এবং স্ক্রিপ্ট এর সমন্বয়ে গঠিত যেটা হোস্ট কনটেইনারগুলোকে একটি প্রোডাকশন ইনভাইরনমেন্টে সহায়তা করতে পারে। মূলত কনটেইনার অর্কেস্ট্রেশন মাল্টিপল ডকার হোস্টের সমন্বয়ে গঠিত যেটা কনটেইনার হোস্ট করতে পারে এমনভাবে যাতে একটি কনটেইনার ফেইল করলেও অন্য কনটেইনার দিয়ে অ্যাপলিকেশনটি অ্যাক্সেস করা যায়। কনটেইনার অর্কেস্ট্রেশন সলিউশ্যান একটি সিঙ্গেল কম্যান্ডের মাধ্যমে যেকোন অ্যাপলিকেশনের শত শত ইন্সট্যান্স রান করতে সক্ষম। 

![](Screenshot%20from%202022-05-15%2016-41-11.png)

মূলত ডকার সোয়ার্মের জন্য এই কম্যান্ড ব্যবহৃত হয়। যখন ইউজার বেড়ে যায় বা কমে যায় তখন কিছু অর্কেস্ট্রেশন সলিউশ্যান অটোমেটিক্যালি ইন্সট্যান্সের সংখ্যা বৃদ্ধি বা কমানোর মাধ্যমে আমাদের সহায়তা করতে পারে। এমনকি কিছু সলিউশ্যান ইউজার লোড বৃদ্ধি পেলে অটোমেটিক্যালি নতুন হোস্ট যোগ করে এবং বিভিন্ন হোস্টের কনটেইনারগুলোর মধ্যে অ্যাডভান্সড নেটওয়ার্কিং এর ব্যবস্থা করে লোড ব্যালেন্সিং-এ সহায়তা করতে পারে। এছাড়াও বিভিন্ন হোস্টের মধ্যে স্টোরেজ শেয়ারিং, কনফিগারেশন ম্যানেজমেন্ট এবং সিকিউরিটির ব্যবস্থা করে দিতে পারে। 

![](Screenshot%20from%202022-05-15%2016-41-49.png)

বর্তমানে এরকম বিভিন্ন ধরনের কনটেইনার অর্কেস্ট্রেশন সলিউশ্যান অ্যাভেইলেবল আছে। যেমন ডকারের আছে docker swarm, গুগলের আছে kubernetes এবং অ্যাপাচির আছে mesos। 

![](Screenshot%20from%202022-05-15%2016-42-54.png)

docker swarm সেট-আপ করা খুব সহজ কিন্তু কমপ্লেক্স প্রোডাকশন অ্যাপলিকেশনের জন্য অ্যাডভান্স ফিচারগুলোর কিছু অভাব রয়েছে এখানে। অন্যদিকে mesos সেট-আপ করা কঠিন কিন্তু অনেক অ্যাডভান্স ফিচার সাপোর্ট করে। kubernetes হলো সবচেয়ে জনপ্রিয়। এটি সেট-আপ করা একটু কঠিন কিন্তু ডিপ্লয়মেন্ট কাস্টোমাইজের জন্য অনেক অপশন প্রদান করে এবং বিভিন্ন ভেনডরস ক্রেডিট যেগুলো সকল পাবলিক ক্লাউড সার্ভিস প্রোভাইডার যেমন gcp, azure, aws তে সাপোর্ট করে সেগুলোর জন্য সাপোর্ট প্রদান করে।
