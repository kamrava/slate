---
title: راهنمای برنامه‌نویسان

language_tabs: # must be one of https://git.io/vQNgJ
  - json
  - xml

toc_footers:
  - <a href='/register'>ثبت نام در سامانه صبا نوین</a>

includes:
  - status

search: false
---

# مقدمه
به راهنمای وب‌سرویس REST سامانه‌ی صبا نوین خوش آمدید.

در این راهنما در مورد چگونگی کار با وب‌سرویس REST سامانه صبا نوین صحبت خواهیم کرد.

در ابتدا باید بگوییم که در وب‌سرویس REST تمامی درخواست‌ها در قالب یک HTTP(S) Request انجام می‌شود.

فرمت کلی هر درخواست به شکل زیر است:

`https://api.sabanovin.com/v1/YOUR-API-KEY/Region/MethodName.OutputFormat`

خروجی هر دستور بسته به نیاز شما می‌تواند با فرمت `JSON` یا `XML` باشد که باید نام فرمت دلخواه را در انتهای هر درخواست قرار دهید.

# احراز هویت

> مثال:

<pre class="link lang-specific json">https://api.sabanovin.com/v1/YOUR-API-KEY/account/balance.json</pre>
<pre class="link lang-specific xml">https://api.sabanovin.com/v1/YOUR-API-KEY/account/balance.xml</pre>

> در تمامی مثال‌ها بجای `YOUR-API-KEY` باید API-Key واقعی خود را قرار دهید.

وب‌سرویس REST سامانه صبا نوین از API-Key برای وصل شدن به سیستم API خود استفاده می‌کند.

 ابتدا شما باید یک API-Key از بخش [بخش وب‌سرویس سامانه](https://sms.sabanovin.com/web_services/rest) ایجاد کنید و سپس در تمامی درخواست‌ها باید API-Key خود را در آدرس URL وارد نمایید.


`https://api.sabanovin.com/v1/YOUR-API-KEY/Region/method-name`

<aside class="notice">
بجای <code>YOUR-API-KEY</code> باید API-Key واقعی خود را قرار دهید.
</aside>

# متدها

## ارسال پیامک تکی و گروهی
`[POST, GET]`
> لینک ارسال درخواست:

<pre class="link lang-specific json">https://api.sabanovin.com/v1/YOUR-API-KEY/sms/send.json?gateway=500043340673&to=09373889007,09176097612&text=تست</pre>
<pre class="link lang-specific xml">https://api.sabanovin.com/v1/YOUR-API-KEY/sms/send.xml?gateway=500043340673&to=09373889007,09176097612&text=تست</pre>

> خروجی درخواست بالا بصورت زیر است:

```json
{
  "batch_id": 354854211,
  "entries": [
    {
      "reference_id": 326545,
      "number": "9373889007",
      "status": "ENQUEUED",
      "datetime": "2017-10-31 11:02:31"
    },
    {
      "reference_id": 326546,
      "number": "9176097612",
      "status": "ENQUEUED",
      "datetime": "2017-10-31 11:02:31"
    }
  ],
  "status": {
    "code": 200,
    "message": "تایید شد"
  }
}
```

```xml
<response>
   <batch_id>354854211</batch_id>
   <entries>
      <item>
         <reference_id>326545</reference_id>
         <number>9373889007</number>
         <status>ENQUEUED</status>
         <datetime>2017-10-31 13:22:39</datetime>
      </item>
      <item>
         <reference_id>326546</reference_id>
         <number>9176097612</number>
         <status>ENQUEUED</status>
         <datetime>2017-10-31 13:22:39</datetime>
      </item>
   </entries>
   <status>
      <code>200</code>
      <message>تایید شد</message>
   </status>
</response>
```

از این متد برای ارسال پیامک به یک یا چند شماره استفاده نمایید.

### پارامتر‌های ورودی

<table class="custom">
  <thead>
    <tr>
      <th>نام</th>
      <th>نوع</th>
      <th>الزامی</th>
      <th>توضیحات</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>gateway</td>
      <td><code>Numeric</code></td>
      <td><span class="fa fa-check-circle-o font-red"></span></td>
      <td>شماره درگاه (شماره خط)</td>
    </tr>
    <tr>
      <td>to</td>
      <td><code>Numeric</code></td>
      <td><span class="fa fa-check-circle-o font-red"></span></td>
      <td>گیرندگان</td>
    </tr>
    <tr>
      <td>text</td>
      <td><code>String</code></td>
      <td><span class="fa fa-check-circle-o font-red"></span></td>
      <td>متن پیام</td>
    </tr>
    <tr>
      <td>text</td>
      <td><code>String</code></td>
      <td><span class="fa fa-minus font-green"></span></td>
      <td>زمان ارسال</td>
    </tr>
  </tbody>
</table>

<aside class="notice">
برای تعداد بیش از یک گیرنده، شماره ها را با کاما <code>,</code> از هم جدا کنید.
</aside>
<aside class="notice">
برای ارسال پیام در آینده تاریخ را بصورت شمسی همراه با زمان وارد کنید.<br> مثال: <code>14:20 1398/10/10</code>
</aside>

### پارامتر‌های خروجی

<table class="custom">
  <thead>
    <tr>
      <th>نام</th>
      <th>توضیحات</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>batch_id</td>
      <td>شناسه دسته پیامک(گیرندگان بیش از یک شماره)</td>
    </tr>
    <tr>
      <td>reference_id</td>
      <td>شناسه پیامک</td>
    </tr>
    <tr>
      <td>number</td>
      <td>گیرنده</td>
    </tr>
    <tr>
      <td>status</td>
      <td>وضعیت</td>
    </tr>
    <tr>
      <td>datetime</td>
      <td>تاریخ ارسال (میلادی)</td>
    </tr>
  </tbody>
</table>

<aside class="notice">
بصورت پیش‌فرض مقدار datetime تاریخ میلادی خواهد بود. اگر مایل به دریافت تاریخ شمسی(جلالی) هستید، باید در header درخواستی مقدار <code>Jalali-DateTime</code> را برابر با 1 قرار دهید.
</aside>

<aside class="success">
برای مشاهده جدول وضعیت‌ها به بخش جدول وضعیت مراجعه کنید.
</aside>

## ارسال پیامک نظیر به نظیر
`[POST]`
> لینک ارسال درخواست:

<pre class="link lang-specific json">https://api.sabanovin.com/v1/YOUR-API-KEY/sms/send_array.json</pre>
<pre class="link lang-specific xml">https://api.sabanovin.com/v1/YOUR-API-KEY/sms/send_array.xml</pre>

> پارامتر‌های ارسالی:

<pre class="link">
gateway = 500043340673
to      = ["093880000", "09120000"]
text    = ["صبا نوین" ,"سامانه پیامکی"]
</pre>


> خروجی درخواست بالا بصورت زیر است:

```json
{
  "batch_id": 354854211,
  "entries": [
    {
      "reference_id": 326545,
      "number": "9373889007",
      "status": "ENQUEUED",
      "datetime": "2017-10-31 11:02:31"
    },
    {
      "reference_id": 326546,
      "number": "9176097612",
      "status": "ENQUEUED",
      "datetime": "2017-10-31 11:02:31"
    }
  ],
  "status": {
    "code": 200,
    "message": "تایید شد"
  }
}
```

```xml
<response>
   <batch_id>354854211</batch_id>
   <entries>
      <item>
         <reference_id>326545</reference_id>
         <number>9373889007</number>
         <status>ENQUEUED</status>
         <datetime>2017-10-31 13:22:39</datetime>
      </item>
      <item>
         <reference_id>326546</reference_id>
         <number>9176097612</number>
         <status>ENQUEUED</status>
         <datetime>2017-10-31 13:22:39</datetime>
      </item>
   </entries>
   <status>
      <code>200</code>
      <message>تایید شد</message>
   </status>
</response>
```

 از متد برای ارسال پیامک بصورت نظیر به نظیر(هر متن متعلق به یک شماره) استفاده کنید. در این متد نحوه ارسال درخواست کمی متفاوت‌تر است. شما باید شماره گیرندگان را بصورت یک آرایه و همینطور متن‌ها را نیز بصورت یک آرایه جداگانه ارسال کنید.

 <aside class="notice">
توجه داشته باشید که تعداد خانه‌های آرایه باید باهم برابر باشد. مثلا اگر تعداد  گیرندگان ۵ شماره است، تعداد متن‌ها نیز باید ۵ متن باشد.
</aside>

### پارامتر‌های ورودی
<table class="custom">
  <thead>
    <tr>
      <th>نام</th>
      <th>نوع</th>
      <th>الزامی</th>
      <th>توضیحات</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>gateway</td>
      <td><code>Numeric</code></td>
      <td><span class="fa fa-check-circle-o font-red"></span></td>
      <td>شماره درگاه (شماره خط)</td>
    </tr>
    <tr>
      <td>to</td>
      <td><code>Array</code></td>
      <td><span class="fa fa-check-circle-o font-red"></span></td>
      <td>گیرندگان</td>
    </tr>
    <tr>
      <td>text</td>
      <td><code>Array</code></td>
      <td><span class="fa fa-check-circle-o font-red"></span></td>
      <td>متن پیام</td>
    </tr>
  </tbody>
</table>

### پارامتر‌های خروجی

<table class="custom">
  <thead>
    <tr>
      <th>نام</th>
      <th>توضیحات</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>batch_id</td>
      <td>شناسه دسته پیامک(گیرندگان بیش از یک شماره)</td>
    </tr>
    <tr>
      <td>reference_id</td>
      <td>شناسه پیامک</td>
    </tr>
    <tr>
      <td>number</td>
      <td>گیرنده</td>
    </tr>
    <tr>
      <td>status</td>
      <td>وضعیت</td>
    </tr>
    <tr>
      <td>datetime</td>
      <td>تاریخ ارسال (میلادی)</td>
    </tr>
  </tbody>
</table>

<aside class="notice">
بصورت پیش‌فرض مقدار datetime تاریخ میلادی خواهد بود. اگر مایل به دریافت تاریخ شمسی(جلالی) هستید، باید در header درخواستی مقدار <code>Jalali-DateTime</code> را برابر با 1 قرار دهید.
</aside>

<aside class="success">
برای مشاهده جدول وضعیت‌ها به بخش جدول وضعیت مراجعه کنید.
</aside>

## دریافت وضعیت
`[POST, GET]`

> لینک ارسال درخواست:

<pre class="link lang-specific json">https://api.sabanovin.com/v1/YOUR-API-KEY/sms/status.json?batch_id=44</pre>
<pre class="link lang-specific xml">https://api.sabanovin.com/v1/YOUR-API-KEY/sms/status.xml?batch_id=44</pre>

> خروجی درخواست بالا بصورت زیر است:

```json
{
  "entries": [
    {
      "reference_id": 187,
      "number": "9373889007",
      "status": "ENQUEUED",
      "datetime": "2017-10-31 14:34:21"
    },
    {
      "reference_id": 188,
      "number": "9176097612",
      "status": "ENQUEUED",
      "datetime": "2017-10-31 14:34:21"
    }
  ],
  "status": {
    "code": 200,
    "message": "تایید شد"
  }
}
```

```xml
<response>
   <entries>
      <item>
         <reference_id>187</reference_id>
         <number>9373889007</number>
         <status>ENQUEUED</status>
         <datetime>2017-10-31 14:34:21</datetime>
      </item>
      <item>
         <reference_id>188</reference_id>
         <number>9176097612</number>
         <status>ENQUEUED</status>
         <datetime>2017-10-31 14:34:21</datetime>
      </item>
   </entries>
   <status>
      <code>200</code>
      <message>تایید شد</message>
   </status>
</response>
```

از این متد برای دریافت وضعیت پیام استفاده کنید.

### پارامتر‌های ورودی

<table class="custom">
  <thead>
    <tr>
      <th>نام</th>
      <th>نوع</th>
      <th>الزامی</th>
      <th>توضیحات</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>batch_id</td>
      <td><code>Numeric</code></td>
      <td><span class="fa fa-check-circle-o font-red"></span></td>
      <td>شماره دسته پیامک</td>
    </tr>
    <tr>
      <td>reference_id</td>
      <td><code>Numeric</code></td>
      <td><span class="fa fa-check-circle-o font-red"></span></td>
      <td>شناسه پیامک</td>
    </tr>
  </tbody>
</table>

<aside class="notice">
فقط یکی از پارامترهای فوق الزامیست.
</aside>

### پارامتر‌های خروجی

<table class="custom">
  <thead>
    <tr>
      <th>نام</th>
      <th>توضیحات</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>reference_id</td>
      <td>شناسه پیامک</td>
    </tr>
    <tr>
      <td>number</td>
      <td>گیرنده</td>
    </tr>
    <tr>
      <td>status</td>
      <td>وضعیت</td>
    </tr>
    <tr>
      <td>datetime</td>
      <td>تاریخ ارسال (میلادی)</td>
    </tr>
  </tbody>
</table>

<aside class="notice">
بصورت پیش‌فرض مقدار datetime تاریخ میلادی خواهد بود. اگر مایل به دریافت تاریخ شمسی(جلالی) هستید، باید در header درخواستی مقدار <code>Jalali-DateTime</code> را برابر با 1 قرار دهید.
</aside>

<aside class="success">
برای مشاهده جدول وضعیت‌ها به بخش جدول وضعیت مراجعه کنید.
</aside>

## دریافت پیامک
`[POST, GET]`

> لینک ارسال درخواست:

<pre class="link lang-specific json">https://api.sabanovin.com/v1/YOUR-API-KEY/sms/receive.json?gateway=500043340673&is_read=0</pre>
<pre class="link lang-specific xml">https://api.sabanovin.com/v1/YOUR-API-KEY/sms/receive.xml?gateway=500043340673&is_read=0</pre>

> خروجی درخواست بالا بصورت زیر است:

```json
{
  "entries": [
    {
      "reference_id": 1244590,
      "number": "0937956770",
      "message": "سلام متن یک",
      "datetime": "2017-11-02 10:28:19"
    },
    {
      "reference_id": "185654",
      "number": "0937762994",
      "message": "سلام متن دو",
      "datetime": "2017-10-27 10:28:19"
    },
    {
      "reference_id": 2454545,
      "number": "0937287124",
      "message": "سلام متن سه",
      "datetime": "2017-10-22 10:28:19"
    },
    {
      "reference_id": 3454904,
      "number": "0937160906",
      "message": "سلام متن چهار",
      "datetime": "2017-11-17 10:28:19"
    }
  ],
  "status": {
    "code": 200,
    "message": "تایید شد"
  }
}
```

```xml
<response>
   <entries>
      <item>
         <reference_id>1244590</reference_id>
         <number>0937956770</number>
         <message>سلام متن یک</message>
         <datetime>2017-11-02 10:28:19</datetime>
      </item>
      <item>
         <reference_id>185654</reference_id>
         <number>0937762994</number>
         <message>سلام متن دو</message>
         <datetime>2017-10-27 10:28:19</datetime>
      </item>
      <item>
         <reference_id>2454545</reference_id>
         <number>0937287124</number>
         <message>سلام متن سه</message>
         <datetime>2017-10-22 10:28:19</datetime>
      </item>
      <item>
         <reference_id>3454904</reference_id>
         <number>0937160906</number>
         <message>سلام متن چهار</message>
         <datetime>2017-11-17 10:28:19</datetime>
      </item>
   </entries>
   <status>
      <code>200</code>
      <message>تایید شد</message>
   </status>
</response>
```

از این متد برای دریافت پیامک‌های دریافتی استفاده کنید.

### پارامتر‌های ورودی

<table class="custom">
  <thead>
    <tr>
      <th>نام</th>
      <th>نوع</th>
      <th>الزامی</th>
      <th>توضیحات</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>gateway</td>
      <td><code>Numeric</code></td>
      <td><span class="fa fa-check-circle-o font-red"></span></td>
      <td>شماره درگاه(شماره خط)</td>
    </tr>
    <tr>
      <td>from_date</td>
      <td><code>DateTime</code></td>
      <td><span class="fa fa-minus font-green"></span></td>
      <td>دریافت شده از تاریخ</td>
    </tr>
    <tr>
      <td>to_date</td>
      <td><code>DateTime</code></td>
      <td><span class="fa fa-minus font-green"></span></td>
      <td>دریافت شده از تاریخ</td>
    </tr>
    <tr>
      <td>is_read</td>
      <td><code>Boolean</code></td>
      <td><span class="fa fa-minus font-green"></span></td>
      <td>خوانده شده یا نشده(پیش‌فرض خوانده نشده)</td>
    </tr>
  </tbody>
</table>

<h3>پیام "خوانده شده" چیست؟</h3>
پیام‌های خوانده‌شده پیام‌هایی هستند که قبلا یا از طریق وب‌سرویس و یا از طریق وب‌هوک آنها را دریافت کرده‌اید. برای دریافت پیام‌های خوانده شده باید پارامتر <code>is_read=1</code> قرار دهید.

<aside class="notice">
مقادیر دو پارامتر "from_date" و "to_date" باید در قالب تاریخ میلادی یا شمسی باشد. مانند:
<p style="text-align: left; direction: ltr;">
2017-10-10
<br>
2017-10-10 21:45:20
</p>
یا
<p style="text-align: left; direction: ltr;">
1396/12/10
<br>
1396/12/10 21:45:20
</p>
</aside>

### پارامتر‌های خروجی

<table class="custom">
  <thead>
    <tr>
      <th>نام</th>
      <th>توضیحات</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>reference_id</td>
      <td>شناسه پیامک</td>
    </tr>
    <tr>
      <td>number</td>
      <td>گیرنده</td>
    </tr>
    <tr>
      <td>datetime</td>
      <td>تاریخ ارسال (میلادی)</td>
    </tr>
  </tbody>
</table>

<aside class="notice">
بصورت پیش‌فرض مقدار datetime تاریخ میلادی خواهد بود. اگر مایل به دریافت تاریخ شمسی(جلالی) هستید، باید در header درخواستی مقدار <code>Jalali-DateTime</code> را برابر با 1 قرار دهید.
</aside>

## دریافت مانده اعتبار
`[POST, GET]`

> لینک ارسال درخواست:

<pre class="link lang-specific json">https://api.sabanovin.com/v1/YOUR-API-KEY/account/balance.json</pre>
<pre class="link lang-specific xml">https://api.sabanovin.com/v1/YOUR-API-KEY/account/balance.xml</pre>

> خروجی درخواست بالا بصورت زیر است:

```json
{
   "entry":{
      "balance":"651173.00"
   },
   "status":{
      "code":200,
      "message":"تایید شد"
   }
}
```

```xml
<response>
   <entry xmlns="https://sms.sabanovin.com" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
      <balance>1999043.70</balance>
   </entry>
   <status xmlns="https://sms.sabanovin.com" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
      <code>200</code>
      <message>تایید شد</message>
   </status>
</response>
```

از این متد برای دریافت اعتبار جاری خود استفاده کنید.

### پارامتر‌های خروجی

<table class="custom">
  <thead>
    <tr>
      <th>نام</th>
      <th>توضیحات</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>balance</td>
      <td>مانده حساب</td>
    </tr>
  </tbody>
</table>

## شماره‌گیر خودکار(افزودن شماره به دفترچه)
`[POST, GET]`

> لینک ارسال درخواست:

<pre class="link lang-specific json">https://api.sabanovin.com/v1/YOUR-API-KEY/utils/add_contact.json?group_id=467&number=09373889007</pre>
<pre class="link lang-specific xml">https://api.sabanovin.com/v1/YOUR-API-KEY/utils/add_contact.json?group_id=467&number=09373889007</pre>

> خروجی درخواست بالا بصورت زیر است:

```json
{
   "entries":[

   ],
   "status":{
      "code":200,
      "message":"شماره مورد نظر با موفقیت به گروه اضافه شد."
   }
}
```

```xml
<response>
   <entries xmlns="https://sms.sabanovin.com" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
      <item />
   </entries>
   <status xmlns="https://sms.sabanovin.com" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
      <code>200</code>
      <message>تایید شد</message>
   </status>
</response>

```

از این متد برای اضافه کردن یک شماره موبایل جدید به دفترچه تلفن استفاده کنید.

### پارامتر‌های ورودی

<table class="custom">
  <thead>
    <tr>
      <th>نام</th>
      <th>نوع</th>
      <th>الزامی</th>
      <th>توضیحات</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>group_id</td>
      <td><code>Numeric</code></td>
      <td><span class="fa fa-check-circle-o font-red"></span></td>
      <td>شناسه گروه دفترچه تلفن</td>
    </tr>
    <tr>
      <td>number</td>
      <td><code>Numeric</code></td>
      <td><span class="fa fa-check-circle-o font-red"></span></td>
      <td>شماره موبایل برای درج در دفترچه تلفن</td>
    </tr>
    <tr>
      <td>fname</td>
      <td><code>String</code></td>
      <td><span class="fa fa-minus font-green"></span></td>
      <td>نام</td>
    </tr>
    <tr>
      <td>lname</td>
      <td><code>String</code></td>
      <td><span class="fa fa-minus font-green"></span></td>
      <td>asd</td>
    </tr>
    <tr>
      <td>gateway</td>
      <td><code>Numeric</code></td>
      <td><span class="fa fa-minus font-green"></span></td>
      <td>نام خانوادگی</td>
    </tr>
    <tr>
      <td>draft_id</td>
      <td><code>Numeric</code></td>
      <td><span class="fa fa-minus font-green"></span></td>
      <td>شناسه پیش‌نویس</td>
    </tr>
  </tbody>
</table>

<aside class="notice">
اگر میخواهید پس از درج شماره موبایل به دفترچه تلفن، پیامی به همان شماره ارسال کنید، باید دو مقدار <code>gateway</code> و <code>draft_id</code> را نیز به پارامترهای ورودی خود اضافه کنید.
</aside>

# خطاها

## خطاهای عمومی

<table class="custom">
  <thead>
    <tr>
      <th>کد خطا</th>
      <th>توضیحات</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>401</td>
      <td>کد شناسه(API-KEY) وب سرویس نامعتبر</td>
    </tr>
    <tr>
      <td>403</td>
      <td>آدرس IP غیر مجاز</td>
    </tr>
    <tr>
      <td>400</td>
      <td>پارامترها ناقص هستند</td>
    </tr>
  </tbody>
</table>

## خطاهای متد ارسال(تکی و گروهی)

<table class="custom">
  <thead>
    <tr>
      <th>کد خطا</th>
      <th>توضیحات</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>400</td>
      <td>پارامترها ناقص هستند</td>
    </tr>
    <tr>
      <td>401</td>
      <td>اعتبار کافی نیست</td>
    </tr>
    <tr>
      <td>411</td>
      <td>گیرنده نامعتبر</td>
    </tr>
    <tr>
      <td>422</td>
      <td>درگاه ارسالی نامعتبر است</td>
    </tr>
  </tbody>
</table>

## خطاهای متد ارسال(نظیر به نظیر)

<table class="custom">
  <thead>
    <tr>
      <th>کد خطا</th>
      <th>توضیحات</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>400</td>
      <td>پارامترها ناقص هستند</td>
    </tr>
    <tr>
      <td>401</td>
      <td>اعتبار کافی نیست</td>
    </tr>
    <tr>
      <td>411</td>
      <td>گیرنده نامعتبر</td>
    </tr>
    <tr>
      <td>422</td>
      <td>تعداد گیرندگان با تعداد متن برابر نیست</td>
    </tr>
  </tbody>
</table>
