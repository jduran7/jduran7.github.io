---
title: "Connecting physicians and labs in Boston"
comments: true
---

<img src="/assets/laboratory.jpg" alt="Drawing" style="width: 100%; height:auto;"/>

Giving back
-----------

One of the best experiences since joining my current job has been to give back by helping physicians in developing countries who are interested in doing clinical research. 

The quantity and quality of research being done in the developing world, both basic and clinical, is limited in many cases due to lack of funding and poor infrastructure. Many talented investigators end up giving up their dreams of contributing to the field and find themselves being forced to work exclusively as clinicians (seeing patients).

<img src="/assets/iri.png" alt="Drawing" style="width: 50%; height:auto;"/>

The **International Research Initiative (IRI)** tries to tackle this problem by recruiting talented physicians who are fluent in English and helping them find a lab in Boston that fits their academic interests. Candidates are interviewed on a yearly basis, and the whole program is ran by researchers who are also from developing countries.

Strategy
-----------

I joined the IRI team in August 2016 with the purpose of applying my knowledge of information technologies into automating some of the tasks related to the recruitment process as well as expanding the program's outreach.

My number one priority was to create a website through which people could find about IRI and apply to it. The program organizers were relying heavily on email communication that in many cases wasn't strictly necessary, and potential candidates could only find about the program via word of mouth.

After working on the website during my free time for some weeks, [irinitiative.com](http://irinitiative.com) was officialy launched on September 2016.

Applications on the website are processed through a form, in which candidates fill their data in plain text and then upload the required documents in PDF format. The applications script checks the file format so that only PDF is allowed, and if all the required fields are filled already, sends an email to a corporate Gmail account with the candidates information in plain text using [phpmailer](https://github.com/PHPMailer/PHPMailer), and their application documents as attachments. This way, the admissions committee can use a tool that they are already familiar with (Gmail), and all the applications are kept in an organized way.

Results
-----------

The website has been well received, and our outreach has increased significantly. We have had **4188** visitors from all over the world since the program was created.  

<img src="/assets/map.png" alt="Drawing" style="width: 100%; height: auto; text-align: center;"/>

Here's a summary of our demographics (top visitor countries):

<img src="/assets/countries.png" alt="Drawing" style="width: 100%; height: auto; text-align: center;"/>

If you happen to know a medical student or graduate who might be interested in the program, don't hesitate to tell them about us. We are recruiting candidates on a yearly basis, usually around June/July, expecting them to start working in a lab around March the next year after being accepted.

I'd like to thank [Dmitry Fedosov](http://dimafeng.com) for his help reviewing my code along the process and for introducing me to the concept of version control, which made my life a lot easier.