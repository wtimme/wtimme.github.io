---
title: "From the time that I almost helped people with Corona symptoms"
description: "Or: How I learned to avoid PDF parsing"
date: 2021-02-28T19:22:56+01:00
draft: false
cover:
    image: "/kvhh-covid-check-screenshot.png"
    alt: "Screenshot of the KVHH-Covid-Check web application with the search form visible"
---

In this post, I will talk about how I almost created a web app that helped people
with Covid-19 symptoms while I was in quarantine myself. And why it was shut down eventually.

Back in mid-2020, when the first "wave" of Corona was hitting Germany and people
were getting more and more cautious about personal contact and transmission of
the disease, I had just returned home from a long-planned vacation in Germany's
south, in Bavaria. We had camped in a VW bus on camping grounds, and had made
sure to keep our distance to other people.

## Getting tested

Despite of us being careful, I catched a bug and came down with flu-like
symptoms. Friends that I wanted to meet with got worried, and asked me to get
tested. Back then, it was relatively new that people got tested, so when the
"mobile doctor" arrived at my doorstep with full-on body protection wear, I had
the attention of the whole neighborhood.

![Man wearing a mask and handling a smoker](/stock-photos/pexels-erik-mclean-5688586-720x714.jpg "The doctor, from the perspective of my neighbors")

After the test, I got told to self-quarantine for two days. In addition, the
doctor handed me a sheet of paper with a barcode on it, and told me that my
result was going to be published as part of a multi-page PDF on the page of the
*Kassenärztliche Vereinigung Hamburg (KVHH)*.

## Querying for results

The Corona tests were getting checked by multiple labs across Hamburg. Once or
twice a day, each lab would upload a collection of their results as one big (visual)
table in a PDF file. That means that patients were required to regularly reload
the page in the browser, download the latest set of PDF files and then manually
(read: `STRG` + `F`) search in the dataset, in the hopes of finding a row with
the code that they received from the doctor.

## There's got to be an easier way!

Now, I am not the kind to just complain about stuff. Rather, I work solution-oriented.
Since I had to stay at home due to my self-quarantine, I had 48 hours at my disposal
and decided to put them to use.

I came up with a simple web app (written in Node.js) that allowed patients
to search _all_ PDFs using one single input field. Here is how the app worked:

1. Every hour, a script would scrape the offical website of the KVV for the PDF files, and download them.
2. Another script (powered by [tabula-java](https://github.com/tabulapdf/tabula-java))
   would then extract the tables from the PDF files into CSV files.
3. Afterwards, the website would be updated.

Upon submission of the search form, the web app would look through all of the CSV
data that it had gathered, and search for a match. Easy peasy.

![Screenshot of the web app with a negative test result](/kvhh-covid-check-result-screenshot.png)

## How it broke

![Vintage photo of a wrecked car](/stock-photos/pexels-pixabay-78793-720x486.jpg)

The tech-savvy reader might have already flinched when I mentioned the terms
"PDF" and "parsing", and they were right: Extracting the data from the PDF files
was the Achilles heel of the whole project, and it was reason I had to shut
the service down in the end.

See, even advanced programs like `tabula-java` have their limitations, especially
if the formatting of the tables in the PDF files differs, sometimes even in the
same document.

The KVHH reworked their web page, but kept the different formats and structures
of the PDF files. (Heck, they had more important things to focus on, afterall.)
Still, I'm a bit bitter because of the value that might project could have
brought to patients/Corona testers across Hamburg.

## Aftermath

In the end, I had to shut the project down. I _tried_ supporting all the different
formats, but I was hesitant to publish the information. The whole data was fragile,
and even though the results that the web app showed always linked to the source
PDF file, and even warned people to only trust the official PDFs, I did not want
to risk giving people false hope, or false confidence.

With this project, I learned a few things, though, including
- how to utilize Crontab for retrieving remote data,
- how to crawl HTML with Node.js,
- how to interact with a Java JAR from within Node.js, and
- how to push-and-deploy my code to Uberspace

So, even though the repository is no longer maintained and the web app is shut
down, my experience was the opposite of my test result: positive

You can find [the project on GitHub](https://github.com/wtimme/kvhh-covid-check).

---

Image attributions:
- ["Businessman in mask holding colored bomb in woods"](https://www.pexels.com/photo/businessman-in-mask-holding-colored-bomb-in-woods-5688586/) by [Erik Mclean](https://www.pexels.com/@introspectivedsgn) via [Pexels](https://www.pexels.com/license/)
- ["Vintage Car Wrecked Grayscale Photo"](https://www.pexels.com/photo/vintage-car-wrecked-grayscale-photo-78793/) by [Pixabay](https://www.pexels.com/@pixabay) via [Pexels](https://www.pexels.com/license/)
