# Mailman Archive Scraper

By Phil Gyford <phil@gyford.com>  
v1.3, 2014-10-26

Latest version is available from <http://github.com/philgyford/mailman-archive-scraper/>

These scripts will scrape the archive pages generated by the [Mailman mailing list manager](http://www.gnu.org/software/mailman/index.html) and republish them as files on the local file system. In addition it can optionally do a number of things:

* Create an RSS feed of recent messages.
* Scrape private Mailman archives (if you have a valid email address and password).
* Remove all email addresses from the files (both those in 'phil@gyford.com' and 'phil at gyford dot com' format).
* Replace the URL for the 'more info on this list' links with another.
* Remove one or more levels of quoted emails.
* Search and replace any custom strings you specify.
* Add custom HTML into the `<head></head>` section of the re-published pages.

Why would you want to do this? Three reasons:

1. You want to create your own HTML archive of a mailing list hosted elsewhere.

2. You want to create a public version of a private archive. We hope you have permission to do this of course. The tools mentioned above allow you to do things like anonymise names and phone numbers, remove email addresses, etc.

3. To have an RSS feed of recent messages.

There may be more efficient ways to do this if you have access to the database in which the Mailman archive is stored. If you don't, and can only access the web pages, this script is for you.

This script doesn't store any state locally between sessions so every time it's run it will have to scrape several pages, even if nothing's changed (particularly if you want an RSS feed of n recent messages). There is a half second delay between each fetch of a remote page, which slows things up but will hopefully prevent hammering web servers.

**There are caveats.** I have only tested this with a couple of Mailman archives (one private, one public) and it seems to work fine. I'm sure that some people will find problems with different installations -- unscrapeable HTML, different URLs and filepaths, etc. Feel free to suggest fixes.


## Installation

1. Put the directory containing the MailmanArchiveScraper.py script somewhere you want to run it from.

2. Make a copy of the `MailmanArchiveScraper-example.cfg` file and name it `MailmanArchiveScraper.cfg`.

3. Set the configuration options in that file (see below).

4. Install the required extra python modules:
	* BeautifulSoup <http://www.crummy.com/software/BeautifulSoup/>
	* ClientForm <http://wwwsearch.sourceforge.net/ClientForm/>
	* Mechanize <http://wwwsearch.sourceforge.net/mechanize/>
	* PyRSS2Gen <http://www.dalkescientific.com/Python/PyRSS2Gen.html>

   Best done with [pip](https://pypi.python.org/pypi/pip) and `pip install -r requirements.txt`.

5. Make sure the `MailmanArchiveScraper.py` script is executable (`chmod +x`). And the `MailmanGzTextScraper.py` script if you need that too.


## Configuration

There is help in the configuration file for each setting. The minimum things you'll need to set are:

1. `domain` -- The domain name that your Mailman pages are on.
2. `list_name` -- Name of your mailing list.
3. `email` and `password` -- Required if your Mailman archive is password protected.
4. `publish_dir` -- The path to the local directory the files should be republished to.
5. `publish_url` -- If you're going to publish the messages to a website.


## Usage

Once configuration is done, run the script:

	$ python ./MailmanArchiveScraper.py

All being well, the HTML archive files will be downloaded. Set the `verbose` setting in the configuration file to see a list of which files are being fetched.

If you want to download the plaintext files that Mailman saves for each month's messages (which may be gzipped), then run this script:

	$ python ./MailmanGzTextScraper.py

After an initial run, you can run the script via cron to keep an updated copy of the HTML and/or text files. Note the `hours_to_go_back` setting in the config file, which wil probably need to be different for the first run compared to subsequent, regular runs.


## What would also be nice:

* Sending each message on as an email. I can't see how to do this simply, given that we retain no state between times the script is run, so can't tell which emails haven't previously been sent.


## Contributors

Many thanks to:

* [CyberRodent](https://github.com/cyberrodent) for the text/gzip file archiving.
* [Danny O'Brien](https://github.com/dannyob) for https support.


## Version history

See all versions: https://github.com/philgyford/mailman-archive-scraper/releases

* v1.3 2014-10-26  
  Add support for non-English language installations.
* v1.2 2013-10-12  
  The monthly text files, possibly gzipped, can be archived using a new script.
* v1.13b 2013-10-12  
  The script can now archive files served over https.
* v1.13 2013-10-12  
  Various improvements to the RSS files generated.
* v1.12 2013-10-12  
  The script can now generate an RSS feed of the most recent posts to the mailing list.
* v1.0 2013-10-12  
  Initial release

  
