# Phishing Analysis 2 — Blue Team Labs Writeup

## Question 1. What is the sending email address?

The first thing I received is an email with the subject "Your Account has been locked" from the sending email address `amazon@zyevantoby.cn`.

![Email Header](https://github.com/user-attachments/assets/3b38e61d-10b9-4f98-a92f-e5c2f52693d2)

## Question 2. What is the recipient email address? (1 point)

The recipient email address is `saintington73@outlook.com`.

## Question 3. What is the subject line of the email? (1 point)

The answer is `Your Account has been locked`.

## Question 4. What company is the attacker trying to imitate? (1 point)

Based on the "from" address, we can see the attacker is trying to imitate **Amazon**.

![Amazon Imitation](https://github.com/user-attachments/assets/02c58cdf-cd81-47c5-95d3-0dbc33126ff3)

## Question 5. What is the date and time the email was sent?

The email was sent on `Wed, 14 Jul 2021 01:40:32 +0900`.

![Email Date](https://github.com/user-attachments/assets/ac7ea61f-0009-4af6-896c-f278ae5ee66d)

## Question 6. What is the URL of the main call-to-action button? (1 point)

To get the malicious URL from the phishing email, I right-clicked the button and selected "Copy Link Location". The URL is:

```
https://emea01.safelinks.protection.outlook.com/?url=https%3A%2F%2Famaozn.zzyuchengzhika.cn%2F%3Fmailtoken%3Dsaintington73%40outlook.com&data=04%7C01%7C%7C70072381ba6e49d1d12d08d94632811e%7C84df9e7fe9f640afb435aaaaaaaaaaaa%7C1%7C0%7C637618004988892053%7CUnknown%7CTWFpbGZsb3d8eyJWIjoiMC4wLjAwMDAiLCJQIjoiV2luMzIiLCJBTiI6Ik1haWwiLCJXVCI6Mn0%3D%7C1000&sdata=oPvTW08ASiViZTLfMECsvwDvguT6ODYKPQZNK3203m0%3D&reserved=0
```

![Copy Link](https://github.com/user-attachments/assets/5f9e80f0-5850-46cf-adfd-07153823f739)

## Question 7. Look at the URL using URL2PNG. What is the first sentence displayed on this site?

I copied the URL above and pasted it into <https://www.url2png.com/>. The first sentence displayed on this site is `This web page could not be loaded`.

![URL2PNG Result](https://github.com/user-attachments/assets/810c34ec-adcb-4206-99f9-57469e12e0e7)

## Question 8. When looking at the main body content in a text editor, what encoding scheme is being used?

I searched for the "Encoding" field in the email source. The encoding scheme being used is `base64`.

![Encoding Field](https://github.com/user-attachments/assets/3ac32ed8-63dc-46e5-8aa3-6b0937fb1261)

![Base64 Content](https://github.com/user-attachments/assets/c1b8da13-c2f0-4ead-b95a-7ddff9b5a94e)

## Question 9. What is the URL used to retrieve the company's logo in the email? (1 point)

The URL used to retrieve the company's logo in the email is:

```
https://images.squarespace-cdn.com/content/52e2b6d3e4b06446e8bf13ed/1500584238342-OX2L298XVSKF8AO6I3SV/amazon-logo?format=750w&content-type=image%2Fpng
```

![Logo URL](https://github.com/user-attachments/assets/90c22f11-5d84-489e-a496-3f0a10b3e879)

## Question 10. For some unknown reason one of the URLs contains a Facebook profile URL. What is the username (not necessarily the display name) of this account, based on the URL?

I guessed that a Facebook profile link would be hidden inside the "Amazon Support Team" section.

![Amazon Support Team](https://github.com/user-attachments/assets/a653751e-f209-4332-945a-61af7d82f261)

After checking the source code, I found the username of this account is `amir.boyka.7`.

![Facebook Username](https://github.com/user-attachments/assets/3a81f626-81e4-44b1-a781-369ac92f4d40)