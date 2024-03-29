এই পর্যায়ে আমরা Docker Architecture সম্পর্কে একটু গভীরভাবে জানব...
---
এটা কিভাবে কাজ করে বিচ্ছিন্ন Container ব্যাবহার করে? ব্যাকগ্রাউন্ডে Docker কি করে আসলে? এসব ব্যাপারে আমরা জানব।

আমরা আগেই জেনেছি, Docker Engine হলো যে হোস্টে আমাদের Docker ইন্সটল করা থাকে। আমরা যখন কোনো লিনাক্স হোস্টে Docker ইন্সটল করছি, তখন বাস্তবে আমরা আসলে ৩ টা আলাদা জিনিস ইন্সটল করছি-
 * Docker Daemon
 * Rest API
 * Docker CLI

```Docker Daemon``` হচ্ছে একটি ব্যাকগ্রাউন্ড প্রসেস যা Docker অবজেক্ট গুলোকে সামলিয়ে থাকে, যেমন, ```Images```, ```Containers```, ```Volumes``` এবং ```Networks```. 

```Rest API``` সার্ভার হচ্ছে API ইন্টারফেস যা প্রোগ্রামগুলো ব্যাবহার করে থাকে ```Docker Daemon``` কে ইন্সট্রাকশন পাঠানোর জন্য। এই ```Rest API``` ব্যাবহার করে আমরা নিজেদের জন্য বিভিন্ন টুল বানিয়ে নিতে পারি।

```Docker CLI``` এর নামটাতেই এর পরিচয় আছে অর্থাৎ এটা হলো সেই ```Command Line Interface```, যেটা ব্যাবহার করে আমরা এখন পর্যন্ত বিভিন্ন Container রান করা, বন্ধ করা, Image ডিলেট করা ইত্যাদি ইত্যাদি কাজ করে আসছি। ```Docker CLI```, ```Rest API``` সার্ভার ব্যাবহার করে ```Docker Daemon``` এর সাথে যোগাযোগ করে থাকে। এটা জরুরি নয় যে ```Docker CLI``` কেও একই হোস্টে হতে হবে। এটা অন্য হোস্টে থেকেও রিমোট ```Docker engine``` হিসেবে কাজ করতে পারে সিম্পলি ```-H``` অপশনটা ব্যাবহার করে। সেক্ষেত্রে কমান্ডে ```-H``` দিয়ে রিমোট ```Docker engine``` এর অ্যাড্রেস ও পোর্ট টা দিতে হবে। 

```docker –H=remote-docker-engine:2375``` 

উদাহরণ হিসেবে, nginx এর উপর বেস করা কোনো Container রিমোট Docker engine এ চালানোর কমান্ড টি হলো,

```docker –H=10.123.2.1:2375 run nginx```

এখানে ```10.123.2.1``` হলো নেটওয়ার্ক অ্যাড্রেস এবং ```2375``` হলো পোর্ট নং।

কন্টেইনারাইজেশন...
---
এখন আমরা দেখব যে কোনো এপ্লিকেশনকে আসলে কিভাবে ```Containerized``` করা হয় Docker এ। Docker সাধারণত ```Namespace``` ব্যাবহার করে ```Workspace``` গুলোকে আলাদা করে থাকে। ```Process ID```, ```Network```, ```interProcess```, ```Mount```, ```Unix Timesharing``` সিস্টেম গুলো তাদের নিজ নিজ ```Namespace``` এ তৈরি করা হয়ে থাকে, যার ফলে Container গুলো সম্পূর্ণ আলাদাভাবে বিচ্ছিন্ন হয়ে চলতে পারে।

![contain](/Docker_engine%2Cstorage/contain.png)

এখন দেখা যাক এমনই একটি Namespace Isolation Technique, যেটা হচ্ছে ```PID``` বা ```Process ID Namespaces```. 

যখন কোনো লিনাক্স সিস্টেম বুট আপ হয়, তখন সেটা শুধু মাত্র একটা প্রসেস নিয়ে শুরু হয় যার ```PID``` হলো 1. এটা হচ্ছে রুট প্রসেস এবং এর পর থেকেই সব প্রসেস শুরু হয়। যতক্ষণে পুরো সিস্টেম টা বুট হওয়া সম্পন্ন হয় ততক্ষণে আরো কয়েকটা প্রসেসের সৃষ্টি হয়ে থাকে। এই প্রসেস গুলোর তালিকা আমরা একটা ```ps``` কমান্ড রান করেই দেখতে পারি। প্রসেস সবগুলোই ইউনিক, অর্থাৎ ২ টি প্রসেস একই ```PID``` ধারী হতে পারে না। এখন আমরা যদি একটি Container সৃষ্টি করি, যেটা আসলে একটা ```Child system```, আসল সিস্টেমের ভেতরে। এই ```Child system``` টার নিজেকে একটি স্বাধীন সিস্টেম হিসেবেই ভাবতে হয়, যার নিচে আরো কিছু প্রসেস থাকবে যেগুলোর রুট প্রসেস হবে সে নিজে এবং তারও ```PID``` শুরু হবে 1 হতে। 

কিন্তু আমরা জানি, Container এবং হোস্টের মাঝে কোনো কঠিন আইসোলেশনের পদ্ধতি নেই। সুতরাং যে প্রসেস গুলো Container এ চলছে, সেগুলো আসলে চলছে হোস্টেই। এই অর্থ দাঁড়ায়, Container এর ```PID``` আবার 1 হতে পারে না, যেহেতু একই PID ২ টি প্রসেসের হতে পারবে না। এখানেই ```Namespace``` এর ব্যাপার টা চলে আসে। ```PID Namespace``` এর মাধ্যমে একটি প্রসেস অনেকগুলো আলাদা ```PID``` পেতে পারে। যেমন, আমাদের Container টি যখন রান হচ্ছে তখন সেটাও পরবর্তী ফাকা ```PID``` টাই পেয়ে যাচ্ছে কিন্তু সাথে এই ```Namespace``` ব্যাবহার করে সে আরেকটা ```PID``` পেয়ে যাচ্ছে যেটা 1 থেকে শুরু হচ্ছে যেটা শুধু ওই Container টাই দেখতে পাবে। এর ফলে Container টা ভাবে যে, সে নিজের জন্য আলাদা রুট প্রসেস ট্রি পেয়ে গিয়েছে অর্থাৎ সে একটি স্বাধীন সিস্টেম। 

![namespace](/Docker_engine%2Cstorage/namespace.png)

এই পুরো ব্যাপারটা আমরা কিভাবে হোস্ট থেকে দেখে বুঝতে পারি বলে মনে হয়?
---
একদম শুরুতে আমরা যে ```nginx``` সার্ভার রান করেছিলাম একটা Container হিসেবে সেটি একটি ```nginx``` সার্ভিস দিচ্ছিলো আমাদের। আমরা যদি Container টির মধ্যকার সকল প্রসেস গুলো লিস্ট করি তাহলে দেখতে পাবো ```nginx``` সার্ভিসটি ```PID 1``` নিয়ে চলছে। এটাই হচ্ছে প্রসেসটার ```PID```, container টির ভেতরে। আমরা যদি হোস্টের মধ্যে এসে আবার এখানকার সব প্রসেসের লিস্ট বের করি তাহলে দেখতে পাবো যে এখই প্রসেসটা অন্য একটা ```PID``` নিয়ে অবস্থান করছে। এর মাধ্যমে এটাই প্রতীয়মান হয় যে প্রত্যেক প্রসেস একই হোস্টের মধ্যে আছে কিন্তু Container গুলো সব আলাদা আলাদা ```Namespace``` এ।
এর থেকে আমরা বুঝতে পারি যে হোস্ট এবং Containers গুলো একই সিস্টেমের রিসোর্স অর্থাৎ ```CPU```, ```Memory``` ব্যাবহার করছে।

cgroups or Control groups...
---

প্রশ্ন আসে যে, কতটা রিসোর্স হোস্ট এবং Containers এর প্রতি এখানে বরাদ্দ? Docker কিভাবে এই রিসোর্সগুলো তার Containers এর মধ্যে ভাগ করে দেয়?

ডিফল্ট হিসেবে Container এর কোনো রিসোর্স লিমিট দেয়া থাকে না অর্থাৎ সেটা চাইলে হোস্টের সব রিসোর্স একাই নিয়ে ফেলতে পারে। কিন্তু সেখানে অবশ্যই ```CPU```  এবং ```Memory``` সীমিত করে দেয়ার অপশনটি রয়েছে। Docker, ```cgroups``` বা ```Control groups``` ব্যবহার করে থাকে এই হার্ডওয়্যার রিসোর্স গুলো কে সীমিত করে দেয়ার জন্য। এটা করা যায় ```--cpus``` অপশনটি ```docker``` কমান্ডের সাথে যুক্ত করে।

```docker run --cpus=.5 ubuntu```

এখানে --cpus=.5 নিশ্চিত করে যে Container টি যেকোনো সময়ে 50% এর বেশি CPU রিসোর্স খরচ করবে না। Memory এর ক্ষেত্রেও একই ঘটনা, এখানে দরকার পড়ে --memory অপশনটি।

```docker run --memory=100m ubuntu```

এই বিষয়ে আরো জানার ইচ্ছা থাকলে নিচে প্রদত্ত লিংক গুলো দেখা যেতে পারে।

* [Docker Engine overview](https://docs.docker.com/engine/)

* [Develop with Docker Engine API](https://docs.docker.com/engine/api/)

* [Control groups](https://docs.docker.com/config/containers/runmetrics/#control-groups)