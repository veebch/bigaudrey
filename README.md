![Action Shot](/images/river.jpg)  

[![YouTube Channel Views](https://img.shields.io/youtube/channel/views/UCz5BOU9J9pB_O0B8-rDjCWQ?style=flat&logo=youtube&logoColor=red&labelColor=white&color=ffed53)](https://www.youtube.com/channel/UCz5BOU9J9pB_O0B8-rDjCWQ) [![GitHub Stars](https://img.shields.io/github/stars/veebch?style=flat&logo=github&logoColor=black&labelColor=white&color=ffed53)](https://github.com/veebch) [![Instagram](https://img.shields.io/badge/Instagram-@v_e_e_b-ffed53?style=flat&logo=instagram&logoColor=white)](https://www.instagram.com/v_e_e_b/)

# Big Audrey: Quiet Internet Content on HD E-Paper  

Big Audrey fetches content tailored to your interests from the internet and displays it in crisp fonts on a 6-inch HD e-paper screen. The script supports multiple display functions, which can be combined and cycled at customizable intervals:  

- **Cryptocurrency/Stock Dashboard** (via [yfinance](https://github.com/ranaroussi/yfinance))  
- **Quotes** (from Reddit's [r/quotes](https://reddit.com/r/quotes))  
- **Word of the Day** (from [Wordsmith.org](https://wordsmith.org))  
- **Headlines** (from any RSS feed, with QR code links to articles)  
- **Cartoons** (Poorly Drawn Lines ©' Reza Farazmand)  
- **Stoic Quotes** (from [Stoic-Quotes.com](https://stoic-quotes.com))  

## Features  

### 📊 Finance Dashboard  
Displays 2-4 cryptocurrencies/stocks per screen. If more are than that number are configured, then the screen cycles through them in a carousel.  

### 💬 Quotes  
Parses and formats quotes from [r/quotes](https://reddit.com/r/quotes), leveraging Reddit's karma system to surface high-quality, relevant content.  

*Why Reddit?*  
The upvote system ensures quotes are interesting and topical. The script includes regex cleanup to minimize garbled outputs.  

### 📖 Word of the Day  
Learn a new word daily and impress everyone with your vocabulary.  

### 📰 Headlines  
Pulls headlines from any RSS feed specified in `config.yaml`.  

### 🎨 Cartoons  
Displays cartoons Poorly Drawn Lines cartoon feed.  

### 🏛️ Stoic Quotes  
Daily wisdom from [Stoic-Quotes.com](https://stoic-quotes.com).  

## Installation  

### Prerequisites  
- Raspberry Pi (set up via [Raspberry Pi Imager](https://www.raspberrypi.com/software/))
   - Hostname: `bigaudrey`  
   - User: `pi`
- HD Epaper Screen ([Waveshare Screen](https://www.waveshare.com/product/6inch-hd-e-paper-hat.htm))

### Setup  
1. **SSH into the Pi** and create a virtual environment:  
   ```bash
   python3 -m venv audrey
   source audrey/bin/activate
   ```  

2. **Clone this repository and turn on SPI**:  
   ```bash
   git clone https://github.com/veebch/bigaudrey.git
   cd bigaudrey
   sudo raspi-config nonint do_spi 0
   ```  

3. **Install dependencies**:
   
   install [IT8951](https://github.com/GregDMeyer/IT8951) driver for e-paper
   ```bash
   pip install -r requirements.txt
   ```  

5. **Configure**:  
   Copy the example config and edit as needed:  
   ```bash
   cp config_example.yaml data/config.yaml
   nano data/config.yaml  # Edit preferences
   ```  

6. **Run**:  
   ```bash
   python3 veebtick.py
   ```  

## Configuration  

Edit `data/config.yaml` to:  
- Enable/disable modes (boolean flags)  
- Adjust function weights (determines display frequency)  

### Example Weighting:  
```yaml
function:
  - mode: ticker         # Finance dashboard
    weight: 1  
  - mode: redditquotes   # Quotes from Reddit
    weight: 10           # 10x more frequent than ticker
  - mode: wordaday       # Word of the Day
    weight: 1
  # ... (other modes)
```  

### Optional: Web-Based Config Editor  
Run [FileBrowser](https://github.com/filebrowser/filebrowser) for easy editing:  
```bash
filebrowser -r /home/pi/bigaudrey/data -p 8080 -a 0.0.0.0
```  
Access via `http://bigaudrey.local:8080` on any device on the same network as Audrey.  

## Auto-Start (Systemd)  
1. Create a service file:  
   ```bash
   sudo nano /etc/systemd/system/veebtick.service
   ```  
   Paste:  
   ```ini
   [Unit]
   Description=Big Audrey E-Paper Service
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

2. Enable the service:  
   ```bash
   sudo systemctl daemon-reload
   sudo systemctl enable veebtick.service
   sudo systemctl start veebtick.service
   ```  

## Contributing  
Contributions are welcome! Fork the repo, create a feature branch, and submit a pull request.  

## License  
GNU General Public License v3.0  
