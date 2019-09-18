# Introduction

The Chrome User Experience Report (CrUX) provides real world, real user metrics gathered from the millions of Google Chrome users who load billions of websites (including yours) each month. It is _this_ data that Google uses to determine how fast your website is and helps [Google determine Search and Ad rankings](https://www.searchenginejournal.com/mobile-page-speed-changes/272221/) for your site.

In 2018 Google provided public access to the CrUX data using their large scale database BigQuery. At this point you may ask, given that BigQuery and the CrUX.run platform both contain the _exact_ same data from Google, why did [we](https://edgemesh.com/?utm_source=crux.run&utm_medium=tools) go through the (fairly large) engineering effort to develop CrUX.run?

<br />

1. Google's BigQuery database was too slow (55 seconds per site)

2. Google's BigQuery database was too expensive ($25 per customer analysis)

3. Aggregated data tools like Page Speed Insights and GTMetrix did not provide enough detail (Aggregated or No real user metrics included)

4. We wanted to help our customers using [Edgemesh](https://edgemesh.com/?utm_source=crux.run&utm_medium=tools) to see the <i>real</i> impact of our software by pulling data directly from Google's database without having to use BigQuery.

<br />

And thus... CrUX.run was born.  We hope you find CrUX.run helpful, and we look forward any [feedback](https://github.com/edgemesh/crux/issues)!


:wave:
The Edgemesh Team  

<br />
<br />
<br />

If you ❤️ CrUX.run, kindly let us know
<a href="https://twitter.com/intent/tweet?screen_name=edgemeshinc&ref_src=twsrc%5Etfw" class="twitter-mention-button" data-text="Thanks @edgemeshinc for https://CrUX.run. #WebPerf" data-show-count="false">Tweet to @edgemeshinc</a><script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>


®2019 Edgemesh Corporation: [Faster Website Experiences That Accelerate Your Business](https://edgemesh.com/?utm_source=crux.run&utm_medium=tools)

The Chrome User Experience Report datasets provided by Google are licensed under a [Creative Commons Attribution 4.0 International License](https://creativecommons.org/licenses/by/4.0/).
