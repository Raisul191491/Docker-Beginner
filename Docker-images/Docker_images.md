# কন্টেইনারাইজেশন
এখন আমরা Python flask framework ব্যবহার করে ডেভেলপ করা একটি সিম্পল ওয়েব এপ্লিকেশনের কন্টেইনারাইজ করব। এপ্লিকেশনটিকে ম্যানুয়ালি ডেপ্লয় করতে চাইলে নিচের ধাপগুলি পর্যায়ক্রমে সম্পাদন করতে হবে। 
1. OS - Ubuntu
2. Update apt repo
3. Install dependencies using apt
4. Install Python dependencies using pip
5. Copy source code to /opt folder
6. Run the web server using "flask" command

এখন Dockerfile নামে একটি ডকার ফাইল তৈরি করে নিম্নের কমান্ডগুলো তাতে যুক্ত করি।
```
FROM Ubuntu

RUN apt-get update
RUN apt-get install python

RUN pip install flask
RUN pip install flask-mysql

COPY . /opt/source-code

ENTRYPOINT FLASK_APP=/opt/source-code/app.py flask run
```
তারপর নিম্নের কমান্ডটি দিয়ে ডকার ফাইলটি বিল্ড করতে হবে যেখানে -t হল ইমেজটির ট্যাগ। এটা আমাদের সিস্টেমে লোকালি একটি ইমেজ তৈরি করবে।
```
docker build Dockerfile –t user_name/image_name
```
পাবলিক ডকার হাব রেজিস্ট্রিতে ফাইলটি নেয়ার জন্যা নিম্নের docker push কমান্ডটি ব্যবহার করতে হবে।
```
docker push user_name/image_name
```

![img1](https://user-images.githubusercontent.com/61577824/168730670-d3278b45-b419-42bd-bc53-eae291f217d1.png)



এখন ডকার ফাইলের দিকে ভালভাবে খেয়াল করা যাক। এটি একটি টেক্সট ফাইল যা ডকারের জন্য বোধগম্য একটি নির্দিষ্ট ফর্মেটে লেখা থাকে। এটি হল Instruction and argument ফর্মেট। প্রতিটি কমান্ডের শুরুর শব্দ যা বড় হাতের অক্ষরে লেখা তা হল instruction এবং পরের অংশ হল argument.

প্রথম লাইন FROM Ubuntu,  কন্টেইনারটির জন্য base OS নির্দিষ্ট করে। প্রতিটি ডকার ফাইল FROM ইন্সট্রাকশনের মাধ্যমে শুরু হয়। 
RUN কমান্ডগুলোর মাধ্যমে ডিপেন্ডেন্সি ইন্সটল করা হয়। 
COPY কমান্ডের মাধ্যমে লকাল সিস্টেম থেকে ফাইল কপি করে ডকার ইমেজে নেয়া হয়। আমাদের উদাহরণে এপ্লিকেশনটির সোর্স কোডটি current folder এ আছে এবং আমরা COPY কমান্ডের মাধ্যমে সোর্স কোডটি ডকার ইমেজের /opt/source-code লোকেশনে কপি করছি। 
ENTRYPOINT ইন্সট্রাকশনটির মাধ্যমে আমাদের ইমেজ যখন কন্টেইনার হিসেবে রান করবে তখন আমরা কোনো নির্দিষ্ট কমান্ড কন্টেইনারে রান করাতে পারি।

![img2](https://user-images.githubusercontent.com/61577824/168730681-a9b1ebd1-8945-4533-be58-7bdbaf254107.png)

# লেয়ার আর্কিটেকচার 
ডকার ইমেজ বিল্ড করার সময় লেয়ার আর্কিটেকচার মেইনটেন করে। প্রতিটি ইন্সট্রাকশন ডকার ইমেজে নতুন লেয়ার তৈরি করে যা শুধু পূর্ববর্তী লেয়ারের পরিবর্তনগুলো নিয়ে গঠিত হয়। এ কারণে প্রতিটি লেয়ার এর পূর্ববর্তী লেয়ারের চেয়ে সাধারণত সাইজে ছোট হয়। docker history কমান্ডটি রান করে প্রতিটি লেয়ারের সাইজ পাওয়া যায়।
```
docker history user_name/image_name
```
![img3](https://user-images.githubusercontent.com/61577824/168730690-3ad40e80-a800-4e49-b07b-72dd3680c6e6.png)


docker build কমান্ডটি রান করলে প্রতিটি স্টেপ এবং স্টেপের ফলাফল টার্মিনালে দেখা যায়।


![img4](https://user-images.githubusercontent.com/61577824/168730700-40d2dae3-09cd-4256-bc67-2d4f1031e4f4.png)

প্রতিটি লেয়ার cache করা থাকে যাতে কোনো লেয়ার ফেইল করলে ঐ লেয়ার থেকে আবারো কাজ শুরু করা যায় বা নতুন কোনো লেয়ার যোগ করতে চাইলে যাতে একদম শুরু থেকে কাজ করতে না হয়। 


[previous page](https://github.com/Raisul191491/Docker-Beginner/blob/main/Docker-run/Docker_run.md) <--> [next page](https://github.com/Raisul191491/Docker-Beginner/blob/main/Docker-compose/Docker_compose.md)
