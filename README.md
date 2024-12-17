![Action Shot](/images/river.jpg)

[![YouTube Channel Views](https://img.shields.io/youtube/channel/views/UCz5BOU9J9pB_O0B8-rDjCWQ?label=YouTube&style=social)](https://www.youtube.com/channel/UCz5BOU9J9pB_O0B8-rDjCWQ) [![Instagram](https://img.shields.io/badge/Instagram-E4405F?style=social&logo=instagram&logoColor=black)](https://www.instagram.com/v_e_e_b/)

# Big Audrey: Quiet internet content on HD epaper.

Big Audrey pulls the stuff that you've told it you're intersted in from the internet, then displays it in pleasingly crispy fonts on a 6 inch HD epaper screen. The script currently can provide one, or any combination from a number different display functions at an interval you choose:

- Cryptocurrency Dashboard
- Quote (from Reddit [r/quotes](https://reddit.com/r/quotes))
- Word of the day (from [wordsmith.org](https://wordsmith.org))
- Headline (From an RSS feed) (With QR code link to the article)
- Cartoon (From The New Yorker)
- Quote (from [Stoic Quotes](https://stoic-quotes.com))

## Functions

### Cryptocurrency Dashboard

Uses code based on the code we made at [btcticker](http://github.com/llvllch/btcticker). The extra screen size means that two three or four coins can fit on the screen at once. If you've listed more coins than that, it will carousel to show all the coins listed. There is also a maximal mode that will show one coin and an item from and RSS news feed, and a QR code link to that article.

### Quotes

This is a script that parses content from [r/quotes](https://reddit.com/r/quotes) and tidies it up a little to make an ever-changing Quote poster, using content from the hive-mind that is the internet.

The quote is then displayed on the attached [Waveshare 6inch HD ePaper](https://www.waveshare.com/6inch-hd-e-paper-hat.htm).

The reason for using r/quotes rather than a curated database, is that the karma score on reddit is a really good way to ensure quotes that are interesting and topical. 

The quality of the results depends on the adherence to convention in posts to [r/quotes](https://reddit.com/r/quotes) and the quality of the regex in the script. Currently the script is rarely producing garbled quotes, so it's ready for sharing. 

As well as producing quotes, the script occasionally places other content on the epaper - to keep things interesting.

### Word of the day

A word and definition for you to try to shoehorn into conversations, making you look like a boffin.

### Headline

A headline from an RSS feed from a news source specified in the `config.yaml` file.

### Cartoon

A cartoon from The New Yorker RSS feed.

### Daily Stoic Quote

A quote from stoic-quotes.com.

# Prerequisites

- A working Pi with waveshare 6inch HD ePaper attached
- The code to drive the display: [IT8951](https://github.com/GregDMeyer/IT8951) installed

# Installation

Get your RaspiOS up-to-date using
    
    sudo apt-get update
    sudo apt-get upgrade

then, enable the SPI interface and clone this repository using

    cd ~
    sudo raspi-config nonint do_spi 0
    git clone https://github.com/veebch/stonks
    sudo reboot
   
This will reboot the pi. Reconnect to the pi and install the modules needed to run this code, using pip and apt-get:

    python3 -m pip install --upgrade pip  
    python3 -m pip install -r requirements.txt
    sudo apt-get install libatlas-base-dev python3-matplotlib python3-numpy

Make a copy of the example config file and run the code using:
    
    cd ~/stonks
    cp config_example.yaml config.yaml
    python3 cryptotick.py
    
# Configuration

Edit the file config.yaml. Entries are commented to indicate their function. There are boolean values for activation of modes, as well as a function section that lists the functions that are sampled from on each refresh iteration. There is also a weighting of those samples. 

```
function:                           
  mode: crypto,redditquotes, wordaday, newyorkercartoon, guardianheadlines, textfilequotes, stoic
  weight: 1, 10, 1, 1, 1, 0, 1       
```
Means that on each iteration there is a 1:10:1:1:1:1 weighting that the code will choose the functions crypto, redditquotes, wordaday, newyorkercartoon, guardianheadlines and stoic respectively.

# Add Autostart


```
cat <<EOF | sudo tee /etc/systemd/system/btcticker.service
[Unit]
Description=btcticker
After=network.target

[Service]
ExecStart=/usr/bin/python3 -u /home/pi/stonks/cryptotick.py
WorkingDirectory=/home/pi/stonks/
StandardOutput=inherit
StandardError=inherit
Restart=always
User=pi

[Install]
WantedBy=multi-user.target
EOF
```
Now, simply enable the service you just made and reboot
```  
sudo systemctl enable btcticker.service
sudo systemctl start btcticker.service

sudo reboot
```

# Video

A demo of some of the features:

[![video](https://img.youtube.com/vi/Xv8eyp-LJJk/0.jpg)](https://www.youtube.com/watch?v=Xv8eyp-LJJk)

# Hardware

- The code runs on a Rasperry Pi connected to a 6 inch HD waveshare epaper display
- The custom metal frame was made for the project - we've got some assembled units for sale at [veeb.ch](https://www.veeb.ch/store/p/tickerxl)

# Contributing

To contribute, please fork the repository and use a feature branch. Pull requests are welcome.

# Licencing

GNU GENERAL PUBLIC LICENSE Version 3.0

