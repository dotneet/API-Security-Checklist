[English](./README.md) | [繁中版](./README-tw.md) | [简中版](./README-zh.md) | [العربية](./README-ar.md) | [বাংলা](./README-bn.md) | [Deutsch](./README-de.md) | [Ελληνικά](./README-el.md) | [Español](./README-es.md) | [فارسی](./README-fa.md) | [Français](./README-fr.md) | [Indonesia](./README-id.md) | [Italiano](./README-it.md) | [日本語](./README-ja.md) | [한국어](./README-ko.md) | [ລາວ](./README-lo.md) | [Македонски](./README-mk.md) | [മലയാളം](./README-ml.md) | [Монгол](./README-mn.md) | [Nederlands](./README-nl.md) | [Polski](./README-pl.md) | [Português (Brasil)](./README-pt_BR.md) | [Русский](./README-ru.md) | [ไทย](./README-th.md) | [Türkçe](./README-tr.md) | [Українська](./README-uk.md) | [Tiếng Việt](./README-vi.md)

# API सुरक्षा जांच-सूची
अपने API को डिजाइन करने, परीक्षण करने और जारी करने के दौरान सबसे महत्वपूर्ण सुरक्षा प्रतिवाद की जांच सूची|


---

## प्रमाणीकरण (Authentication)
- [ ] `बेसिक एथ` का उपयोग मानक प्रमाणन का उपयोग न करें (जैसे [JWT](https://jwt.io/), [OAuth](https://oauth.net/))।
- [ ] `प्रमाणीकरण`, `टोकन पीढ़ी`, `पासवर्ड भंडारण` में पहिया को फिर से न बदलें। मानकों का उपयोग करें।
- [ ] लॉग इन में `मैक्स पुन: प्रयास` और `जेल` सुविधाओं का उपयोग करें।
- [ ] सभी संवेदनशील डेटा पर एन्क्रिप्शन का उपयोग करें।

### JWT (JSON वेब टोकन)
- [ ] एक यादृच्छिक जटिल कुंजी (`JWT सीक्रेट`) का प्रयोग करें ताकि brute force करने के लिए टोकन बहुत कठिन हो।
- [ ] पेलोड से एल्गोरिदम न निकालें। बैकएण्ड (`HS256` या `RS256`) में एल्गोरिथम को बल दें।
- [ ] टोकन की समाप्ति (`टीटीएल`, `आरटीटीएल`) को यथासंभव कम करें।
- [ ] JWT पेलोड में संवेदनशील डेटा को संचित न करें, इसे [आसानी](https://jwt.io/#debugger-io) से डिकोड किया जा सकता है।
- [ ] ज्यादा डाटा स्टोर करने से बचें। JWT को आमतौर पर headers में साझा किया जाता है और उनकी एक आकार सीमा होती है।

## Access
- [ ] DDOS / ब्रूट-फॉरेस्ट हमलों से बचने के लिए सीमा अनुरोध (थ्रोटलिंग)।
- [ ] MITM (मैन इन द मिडल अटैक) से बचने के लिए सर्वर साइड पर HTTPS का उपयोग करें।
- [ ] SSL strip हमले से बचने के लिए SSL के साथ HSTS हैडर का उपयोग करें।
- [ ] निर्देशिका लिस्टिंग बंद करें।
- [ ] निजी API के लिए, केवल श्वेतसूची वाले IP/होस्ट से ही एक्सेस की अनुमति दें।

## Authorization

### OAuth
- [ ] केवल व्हाइटलिस्ट किए गए URL को अनुमति देने के लिए हमेशा `redirect_uri` सर्वर-पक्ष को मान्य करें।
- [ ] हमेशा कोड के लिए आदान-प्रदान करने की कोशिश नहीं करें और टोकन न दें (`response_type=token` की अनुमति न दें)
- [ ] OAuth प्रमाणीकरण प्रक्रिया पर CSRF को रोकने के लिए एक यादृच्छिक हैश के साथ `state` पैरामीटर का उपयोग करें।
- [ ] डिफ़ॉल्ट स्कोप को परिभाषित करें, और प्रत्येक एप्लिकेशन के लिए स्कोप मापदंडों को मान्य करें।

## Input
- [ ] ऑपरेशन के अनुसार उचित HTTP विधि का प्रयोग करें: अनुरोधित विधि है, अगर `GET (पढ़ें)`, `पोस्ट (बनाएं)`, `पुट / पैच (प्रतिस्थापित / अद्यतन)`, और `हटाएं (रिकॉर्ड को हटाने के लिए)`, और `405 Method Not Allowed` के साथ प्रतिक्रिया न दें अनुरोधित संसाधन के लिए उचित नहीं है
- [ ] अनुरोध पर `content-type` मान्य करें केवल अपने समर्थित प्रारूप (जैसे `application/xml`, `application/json`, आदि) को अनुमति देने के लिए हेडर (सामग्री वार्ता-Content Negotiation) स्वीकार करें और `406 Not Acceptable` करें यदि स्वीकार्य न हो तो।
- [ ] जैसा कि आप स्वीकार करते हैं, उतनी ही पोस्ट की गई `content-type` की पुष्टि करें (जैसे `application/x-www-form-urlencoded`, `multipart/form-data`, `application/json`, इत्यादि)।
- [ ] सामान्य कमजोरियों (जैसे `XSS`, `SQL-Injection`, `Remote Code Execution`, आदि) से बचने के लिए उपयोगकर्ता इनपुट मान्य करें।
- [ ] URL में किसी भी संवेदनशील डेटा (`credentials`, `Passwords`, `security tokens`, या `API keys`) का उपयोग न करें, लेकिन मानक प्राधिकरण शीर्ष लेख का उपयोग करें।
- [ ] केवल सर्वर-साइड एन्क्रिप्शन का उपयोग करें।
- [ ] कैशिंग, दर सीमा नीतियों (`Quota`, `Spike Arrest`, `Concurrent Rate Limit`) को सक्षम करने के लिए API गेटवे सेवा का उपयोग करें और गतिशील रूप से API संसाधनों की तैनाती करें।

## Processing
- [ ] जांचें कि क्या सभी समापन बिंदुओं को टूटा प्रमाणीकरण प्रक्रिया से बचने के लिए प्रमाणीकरण के पीछे सुरक्षित किया गया है या नहीं।
- [ ] उपयोगकर्ता के स्वयं के संसाधन आईडी से बचना चाहिए। `/user/654321/orders` के बजाय `/me/orders` का उपयोग करें।
- [ ] auto-increment आईडी न करें। बजाय यूयूआईडी का प्रयोग करें।
- [ ] यदि आप XML फ़ाइलों को पार्स कर रहे हैं, तो सुनिश्चित करें कि इकाई पार्सिंग XXE (XML external entity attack) से बचने के लिए सक्षम है।
- [ ] यदि आप XML फ़ाइलों को पार्स कर रहे हैं, तो सुनिश्चित करें कि `Billion Laughs/XML bomb` (exponential entity expansion attack) के हमले से बचने के लिए सक्षम है।
- [ ] फ़ाइल अपलोड के लिए CDN का उपयोग करें।
- [ ] यदि आप बड़ी मात्रा में डेटा के साथ काम कर रहे हैं, तो Workers और Queues का उपयोग पृष्ठभूमि में यथासंभव प्रक्रिया करने के लिए और HTTP अवरोधन(Blocking) से बचने के लिए तेज़ी से return response करें।
- [ ] DEBUG मोड बंद करने के लिए मत भूलना।
- [ ] उपलब्ध होने पर गैर-निष्पादन योग्य stack का उपयोग करें।

## Output
- [ ] `X-Content-Type-Options: nosniff` हेडर भेजें।
- [ ] `X-Frame-Options: deny`हेडर भेजें।
- [ ] `Content-Security-Policy: default-src 'none'`हेडर भेजें।
- [ ] `X-Powered-By`, `Server`, `X-AspNet-Version` फिंगरप्रिंटिंग हेडर हटाएं।
- [ ] आपकी प्रतिक्रिया के लिए `content-type` को बल दें, यदि आप `application/json` वापस करते हैं तो आपकी प्रतिक्रिया `content-type` `application/json` है।
- [ ] `credentials`, `Passwords`, `security tokens` जैसे संवेदनशील डेटा वापस न करें।
- [ ] ऑपरेशन के अनुसार उचित स्थिति कोड वापस करें। (जैसे `200 OK`, `400 Bad Request`, `401 Unauthorized`, `405 Method Not Allowed`, आदि)।

## CI & CD
- [ ] unit/integration परीक्षण कवरेज के साथ अपने डिजाइन और कार्यान्वयन की जांच करें।
- [ ] कोड समीक्षा प्रक्रिया का उपयोग करें और स्वयं-स्वीकृति की उपेक्षा करें।
- [ ] सुनिश्चित करें कि आपकी सेवाओं के सभी components को AV सॉफ्टवेयर द्वारा स्कैन करने से पहले उत्पादक को push. vendor libraries और अन्य dependencies शामिल हैं।
- [ ] अपने कोड पर लगातार सुरक्षा परीक्षण (स्थिर/गतिशील विश्लेषण) चलाएं।
- [ ] ज्ञात कमजोरियों के लिए अपनी निर्भरता (सॉफ्टवेयर और ओएस दोनों) की जाँच करें।
- [ ] तैनाती के लिए एक रोलबैक समाधान तैयार करें।

## Monitoring
- [ ] Use centralized logins for all services and components.
- [ ] Use agents to monitor all traffic, errors, requests, and responses.
- [ ] Use alerts for SMS, Slack, Email, Telegram, Kibana, Cloudwatch, etc.
- [ ] Ensure that you aren't logging any sensitive data like credit cards, passwords, PINs, etc.
- [ ] Use an IDS and/or IPS system to monitor your API requests and instances.


---

## यह भी देखें:
- [yosriady/api-development-tools](https://github.com/yosriady/api-development-tools) RESTful HTTP+JSON APIs के निर्माण के लिए उपयोगी संसाधनों का संग्रह।


---

# योगदान
इस रिपोजिटरी contribute, कुछ बदलाव करने और pull request सबमिट करने में योगदान करने के लिए स्वतंत्र महसूस करें। किसी भी प्रश्न के लिए हमें `team@shieldfy.io` पर एक ईमेल है।
