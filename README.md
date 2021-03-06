[![Intro](https://github.com/bokub/nopaste/raw/images/intro.png)][intro-img]

[NoPaste](https://nopaste.ml/) is a client-side paste service which works with **no database**, and **no back-end code**

Instead, the data is **compressed** then **stored** into a unique URL that can be shared and decoded later. For example, [this is the CSS code used by NoPaste][example]

As a result, there is no risk of data being lost, censored or deleted. The data is stored entirely **in the links** and nowhere else!

**Note:** This project is a copy of [Topaz's paste service][topaz-example], with a reworked design and a few additional features (syntax highlighting, line numbers, line wrapping, embedding...)

## How it works

When you click on "Generate Link", NoPaste compresses the whole text using the
[LZMA algorithm](https://en.wikipedia.org/wiki/Lempel%E2%80%93Ziv%E2%80%93Markov_chain_algorithm), encodes it in
[Base64](https://en.wikipedia.org/wiki/Base64), and puts it in the optional URL fragment, after the first `#` symbol: `nopaste.ml/#<your data goes here>`

When you open a link, NoPaste reads, decodes, and decompresses whatever is after the `#`, and displays the result in the editor.

This process is done entirely **in your browser**, and the web server hosting NoPaste [never has access to the fragment](https://en.wikipedia.org/wiki/Fragment_identifier)

## Maximum sizes for links

NoPaste is great for sharing code snippets on various platforms.

These are the maximum link lengths on some apps and browsers.

| App     | Max length |
| ------- | ---------- |
| Reddit  | 10,000     |
| Twitter | 4,088      |
| Slack   | 4,000      |
| QR Code | 2,610      |
| Bitly   | 2,048      |

| Browser         | Max length                | Notes                                   |
| --------------- | ------------------------- | --------------------------------------- |
| Google Chrome   | (win) 32,779 (mac) 10,000 | Will not display, but larger links work |
| Firefox         | >64,000                   |                                         |
| Microsoft IE 11 | 4,043                     | Will not show more than 2,083           |
| Microsoft Edge  | 2,083                     | Anything over 2083 will fail            |
| Android         | 8,192                     |                                         |
| Safari          | Lots                      |                                         |

## Embedded NoPaste snippets

You can include NoPaste code snippets into your own website by clicking the _Embed_ button and using the generated HTML code

[This is what an embedded Paste snippet looks like](https://jsfiddle.net/bokub/nwtcdrph/show)

Feel free to edit the generated `height` and `width` attributes, so they suit your needs

## Generate NoPaste links

NoPaste links can be created easily from your system's command line:

```bash
# Linux
echo -n 'Hello World' | lzma | base64 | xargs -0 printf "https://nopaste.ml/#%s"

# Mac
echo -n 'Hello World' | lzma | base64 -w0 | xargs -0 printf "https://nopaste.ml/#%s"

# Windows / WSL
echo -n 'Hello World' | xz --format=lzma | base64 -w0 | printf "https://nopaste.ml/#%s" "$(cat -)"
```

[intro-img]: https://nopaste.ml/?lang=python#XQAAAQA6BAAAAAAAAAAFYH9Ev4Ly6wIDoAZQ25VXENWodOrWpmx8bfd8j6jeNeL/0fGICEpU6gh9GhXuFjqBBpJFOvefaxUXJquUWRQarmV+S0SHFOLUFyg/dw8OQI6RB6Y1yliOBWGL916HxAGqgwyLqjkH4w450OLk+q9oJYS6PrZfXU5uBC5DLe76OkG25ibvbsasKN51JuVyRedgoF/d7F9d6L7p02q0jHAM9pnPpKOKqlIVsNnXwYKXK10tnZ0GdiwJ3EeT//52uhw=
[example]: https://nopaste.ml/?lang=css#XQAAAQB2DQAAAAAAAAAXioAj/ZZaukKWizx++f8w08qY+xe+w0AeNB0EtEDMR1jPECOrMSz2rcy5XqUVTzusmFXo407ujwufsB1Va3cy02BV4Chx15I+SbM8Ei2WS8/MaZa0wIOjHf0s6B9Kcwi1J73qYeIcKm39PEWGnBM4Ym5aXFOF1Irrn1N95vEcl4YI+98rydudZHmsNCmt995GvCpLImwH7yj3SlMadWaQA0jHCrY1ZBvhjSJ9mAAdYjCJLduITZuXomgpqtr3sYI1WKeRGNmPSD8J8WRLtCx0BZl3yWZZrUxJbmVod8cYiPJnlO+CzQA03qA/GZnxhMYd9TPc2Xlq8UIhBX6gmvo/xhHJc/WHnuBFB32xJ37O5FZlZuXGy6wFE/lakVteoqEqgvyan4LfaiL0pMfYapWjV8YoPa/KyVGugANNjvtRw0hRr+Z1IgKoVL2a1xqvQiB65vIXkw68/ui82O1ko9HTbsLMHX/2/CgWZ8TkTEvgr0+dzVqQYIpaWpB3hDnUTHMkG2UehM5iJyJXAgODNqk8IiWCJn5k/j2FeFpchSTWdgi7AeaCowubmWnFTNgNFXLf5zzARWBUGQFT55TiC0ah4HL17jG3rY6fXvAXlm6CWc88ne7wF0opHkLnhfPslssDYo75hDmCfYwJ6asQ/YBkSuuLJjdCEXVjF7Pdw2FhsOiiB0gNXC6ehieM28M+PMGSDRqt0Q0KveMgE44YJ8zFOvpu2Gg41qDkrsYS7Xb/iMnHz66tt69I1rGzrHx88PuI2/CZx+kv+z7a+/Eiq4JC3KTDx4IbNUYptmrIOC2bxGrcjd8TBKGe+dNi8a/PEnXrUXc/OD7D680/Awo+scE8VKuRVN5R7OPg0tmKVQSQkyCf+I3//kGmREyhh6/bCv6tvbEc7hNPNE1js2svlVBF001JlLY7g1w6ls2pjxrKxuCLrkjba1n4s5WlfrbwcX18rqgbfL2tVibHggsy3Lgq4i7fXtuO3JifxfauO66YIRjEWFnACrHbZ14FbHDMylN7GMvo5TusxestU0s6+kiibWq2qZVpi7C5JVKURNQrGEabICn5nUDoOPMZSfNrdmlTOhu4qjKW09XdQPNazTMc83KjG1YOKNjRP23i1oav5muBFRGMDkSXt7wCgv5PCpLZM9w2jC5GCa5oHuH92IWJu5d80fvTRw3GxNQzfMRxgjzfH6xxrzu+EghhbzayUhb3U5aSRSxOtPTPfjT9mxgk7j3f1mRlbYheuhko4/LBIKYvfA5CnN2Yr3VyBYqoZ/ZgP871LU0ix8ZLeaecao0SDj6V35bZ6RB53mcU8BRPfcyZhj0H6BrFcrcfXKVwnNVro/cIrwnoG3GwlpCXGYJmDDeUdlbBj2HrVmvMncd3w8SzPtxw5RAWQP9YPJdE4Tc490BMX4zAkPqirRHLcxG/K5ECLtKtGsnCkg24+XwFo4XRcGfMsbkSSYD7oZ2HkD+1NXYqJKgk+7uee+yrYCZF6xbsb0ca/7r3w8nU9DueSa6XPiaCLSeJ+phw8iK1U4+FcCzqLW4LzgfcGjz64+HM+Xst0YuqzrAI2RU80H2Mr0FnC/qL+klbM+p0yNUBxBd4IQ64SJmh6Irgi7fZq2wfuTuVEAI1qKKwGwBQ6x9UDyY/OqkD63mtRL//oHeipA==
[topaz-example]: https://topaz.github.io/paste/#XQAAAQADAQAAAAAAAAAFFgvDUiqpf8dDPleMqfsqtbQYE28suCtDTB9iyFgGByXgSRMepMuokjoACV4UPgBzwM3p+V/N2rCi8m90FkQfsRuMJ4LrZVFgr81wKDc2okcywbJBz7OGNPpc8xu2lAkpSekqRO+I/OYUUvgB2ckKBp+2avxmAKn6H73WV3t1D5Ip9hwthecGUnLwFSGpPFNI0zb4Ettx7QnIu6NiftBuencR3Bn/l3BNoh8M5NQL2MoInMQAnQ1gGwSQg6uEnIvK70ERxjllyP2v2fH/N5CRAA==
