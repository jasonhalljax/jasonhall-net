# Jason Hall - Personal Website

A clean, modern personal website built with pure HTML, CSS, and JavaScript. Designed for easy self-hosting on Apache/Ubuntu Server.

## Features

- **No build tools required** - Just HTML, CSS, and JavaScript
- **Responsive design** - Works on all screen sizes
- **Modern aesthetics** - Professional dark theme with accent colors
- **Fast loading** - Minimal dependencies (only Google Fonts)
- **Accessible** - Semantic HTML and keyboard navigation
- **Easy to customize** - Well-organized CSS variables

## Directory Structure

```
jasonhall-website/
├── index.html          # Main HTML file
├── css/
│   └── style.css       # All styles
├── js/
│   └── main.js         # JavaScript functionality
├── assets/             # Images (add your own)
│   ├── headshot.png
│   ├── Python-Symbol.png
│   ├── GitHub-logo.png
│   ├── nginx_lets_encrypt.jpg
│   ├── hobby_homelab.jpg
│   ├── hobby_scuba.jpg
│   ├── hobby_movies.jpg
│   ├── hobby_college_athletics.jpg
│   ├── hobby_biking.jpg
│   └── hobby_lawn_care.jpg
└── README.md
```

## Deployment on Apache (Ubuntu Server)

### 1. Install Apache (if not already installed)

```bash
sudo apt update
sudo apt install apache2
```

### 2. Enable required modules

```bash
sudo a2enmod rewrite
sudo a2enmod headers
sudo systemctl restart apache2
```

### 3. Copy website files

```bash
# Option A: Copy to default web root
sudo cp -r /path/to/jasonhall-website/* /var/www/html/

# Option B: Create a new virtual host directory
sudo mkdir -p /var/www/jasonhall.net
sudo cp -r /path/to/jasonhall-website/* /var/www/jasonhall.net/
```

### 4. Set proper permissions

```bash
sudo chown -R www-data:www-data /var/www/html/
# or for virtual host:
sudo chown -R www-data:www-data /var/www/jasonhall.net/
```

### 5. Configure Virtual Host (optional but recommended)

Create a new virtual host configuration:

```bash
sudo nano /etc/apache2/sites-available/jasonhall.net.conf
```

Add the following content:

```apache
<VirtualHost *:80>
    ServerName jasonhall.net
    ServerAlias www.jasonhall.net
    DocumentRoot /var/www/jasonhall.net
    
    <Directory /var/www/jasonhall.net>
        Options -Indexes +FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>
    
    # Security headers
    Header always set X-Content-Type-Options "nosniff"
    Header always set X-Frame-Options "SAMEORIGIN"
    Header always set X-XSS-Protection "1; mode=block"
    
    # Caching for static assets
    <FilesMatch "\.(css|js|png|jpg|jpeg|gif|ico|woff2?)$">
        Header set Cache-Control "max-age=2592000, public"
    </FilesMatch>
    
    ErrorLog ${APACHE_LOG_DIR}/jasonhall_error.log
    CustomLog ${APACHE_LOG_DIR}/jasonhall_access.log combined
</VirtualHost>
```

Enable the site:

```bash
sudo a2ensite jasonhall.net.conf
sudo a2dissite 000-default.conf  # Disable default if desired
sudo systemctl reload apache2
```

### 6. Enable HTTPS with Let's Encrypt (recommended)

```bash
sudo apt install certbot python3-certbot-apache
sudo certbot --apache -d jasonhall.net -d www.jasonhall.net
```

## Adding Your Images

Replace the placeholder references in `index.html` with your actual images:

1. Copy your images to the `assets/` folder
2. Make sure filenames match what's referenced in `index.html`, or update the `src` attributes

### Required images:
- `headshot.png` - Your profile photo (recommended: 400x400px)
- `Python-Symbol.png` - Python logo for project card
- `GitHub-logo.png` - GitHub logo for project card
- `nginx_lets_encrypt.jpg` - Nginx/Let's Encrypt image for project card
- `hobby_*.jpg` - Hobby section images (recommended: 600x400px)

## Customization

### Colors
Edit the CSS variables in `css/style.css`:

```css
:root {
    --color-primary: #0f172a;       /* Main background */
    --color-accent: #06b6d4;        /* Accent color (cyan) */
    --color-text: #f8fafc;          /* Text color */
    /* ... more variables ... */
}
```

### Fonts
The site uses Google Fonts. To change fonts, edit the `<link>` in `index.html` and update `--font-display`, `--font-body`, and `--font-mono` in CSS.

### Content
All content is in `index.html`. Update:
- Hero section text
- About section paragraphs
- Project cards
- Timeline achievements
- Hobby cards and modals
- Contact links (update the `href` attributes with your actual profiles)

## Browser Support

- Chrome 90+
- Firefox 88+
- Safari 14+
- Edge 90+

## Performance Tips

1. **Optimize images** - Use WebP format and compress images
2. **Enable GZIP** - Add to your Apache config:
   ```apache
   <IfModule mod_deflate.c>
       AddOutputFilterByType DEFLATE text/html text/css application/javascript
   </IfModule>
   ```
3. **Enable HTTP/2** - Modern Apache versions support this automatically with HTTPS

## License

Feel free to use and modify this template for your personal website.

