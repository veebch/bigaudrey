![Action Shot](/images/river.jpg)

[![YouTube Channel Views](https://img.shields.io/youtube/channel/views/UCz5BOU9J9pB_O0B8-rDjCWQ?style=flat&logo=youtube&logoColor=red&labelColor=white&color=ffed53)](https://www.youtube.com/channel/UCz5BOU9J9pB_O0B8-rDjCWQ) [![Instagram](https://img.shields.io/github/stars/veebch?style=flat&logo=github&logoColor=black&labelColor=white&color=ffed53)](https://www.instagram.com/v_e_e_b/)


# Big Audrey: Quiet internet content on HD epaper.

Big Audrey pulls the stuff that you've told it you're intersted in from the internet, then displays it in pleasingly crispy fonts on a 6 inch HD epaper screen. The script currently can provide one, or any combination from a number different display functions at an interval you choose:

- Cryptocurrency/ Stocks Dashboard (using [yfinance](https://github.com/ranaroussi/yfinance))
- Quote (from Reddit [r/quotes](https://reddit.com/r/quotes))
- Word of the day (from [wordsmith.org](https://wordsmith.org))
- Headline (From any RSS feed) (With QR code link to the article)
- Cartoon (From The New Yorker)
- Quote (from [Stoic Quotes](https://stoic-quotes.com))

## Functions

### Finance Dashboard

Two three or four coins/symbols can fit on the screen at once. If you've listed more coins than that, it will carousel to show all the coins/ symbols listed.

### Quotes

This is a script that parses content from [r/quotes](https://reddit.com/r/quotes) and tidies it up a little to make an ever-changing Quote poster, using content from the hive-mind that is the internet.

The reason for using r/quotes rather than a curated database, is that the karma score on reddit is a really good way to ensure quotes that are interesting and topical. 

The quality of the results depends on the adherence to convention in posts to [r/quotes](https://reddit.com/r/quotes) and the quality of the regex in the script. Currently the script is rarely producing garbled quotes.  

### Word of the day

A word and definition for you to try to shoehorn into conversations, making you look like a boffin.

### Headline

A headline from an RSS feed from a news source specified in the `config.yaml` file.

### Cartoon

A cartoon from The New Yorker RSS feed.

### Daily Stoic Quote

A quote from stoic-quotes.com.

# Installation

This is how to build the code from scratch. It means you'll be using the most up-to-date version of the various libraries.

The first thing that needs to be set up is a venv so that we can install the dependencies.

Using the Raspberry Pi imager, copy the latest image to your SD card, add your wifi details and add a user 'pi'. set the hostname to `bigaudrey`.

Now ssh in, and add a venv using the command:

``` python3 -m venv audrey ```

then start the virtual environment using 

``` source audrey/bin/activate ``` 

Install IT8951 ( make sure you use `pip install ./[rpi]`)

clone this repository and cd into the directory with 

```
git clone https://github.com/veebch/bigaudrey.git
cd bigaudrey
```

Install the requirements with 

``` python3 -m pip install -r requirements.txt ```

Copy the example config file:

``` cp config_example.yaml data/config.yaml```

Now you should be able to run the code using 

``` python3 veebtick.py ```
# Configuration

To edit the config file you can either ssh (path: bigaudrey/data).

Edit the file config.yaml. Entries are commented to indicate their function. There are boolean values for activation of modes, as well as a function section that lists the functions that are sampled from on each refresh iteration. There is also a weighting of those samples. 

```
function: # Functions that Audrey has, along with a weighting for each
  - mode: ticker
    weight: 1 
  - mode: redditquotes
    weight: 10
  - mode: wordaday
    weight: 1
  - mode: newyorkercartoon
    weight: 1
  - mode: headlines
    weight: 1    
  - mode: textfilequotes
    weight: 1
  - mode: stoic
    weight: 1     
```
Means that on each iteration there is a 1:10:1:1:1:1:1 weighting that the code will choose the functions ticker, redditquotes, wordaday, newyorkercartoon, headlines textfilequotes and stoic respectively.

### Optional

If you want to easily edit the config file from your phone etc, then install
the excellent [filebrowser](https://github.com/filebrowser/filebrowser)
and point it at the config directory with
`filebrowser -r /home/pi/bigaudrey/data -p 8080 -a 0.0.0.0`. 
You can now access it by pointing any device on the same LAN with a web-browser at `http://bigaudrey.local:8080`

## Automatic Startup 
To use systemd, 
```sudo nano /etc/systemd/system/veebtick.service```

and enter
```
[Unit]
Description=Veebtick Python Service
After=network-online.target
Wants=network-online.target

[Service]
Type=simple
User=pi
WorkingDirectory=/home/pi/bigaudrey
ExecStart=/home/pi/audrey/bin/python /home/pi/bigaudrey/veebtick.py
Restart=on-failure
Environment=PYTHONUNBUFFERED=1

[Install]
WantedBy=multi-user.target
```
Enable the service and start it with:
```
sudo systemctl daemon-reload
sudo systemctl enable veebtick.service
sudo systemctl start veebtick.service
```
# Contributing

To contribute, please fork the repository and use a feature branch. Pull requests are welcome.

# Licencing

GNU GENERAL PUBLIC LICENSE Version 3.0

