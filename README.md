# Interactive 3D Keyboard with Monitor Display

## Overview
This project creates an interactive 3D keyboard and monitor scene where clicking different keyboard keys displays different content (images, videos, HTML pages, games) on the monitor screen.

## Features
- ✨ 3D keyboard with clickable keys
- 🖥️ Monitor display that shows different content per key
- 🎨 Support for images, videos, custom HTML pages, and external websites
- 🎮 Can display interactive content (games, forms, etc.)
- 📱 Responsive iframe that perfectly matches the 3D monitor mesh
- 🔄 Toggle functionality (click same key again to turn off)

## How It Works

### Content Mapping System
Edit the `keyContentMap` object in `index.html` to assign content to different keys:

```javascript
const keyContentMap = {
    'key012': {
        type: 'image',
        src: 'aste_logo.jpg',
        description: 'ASTE Logo'
    },
    'key013': {
        type: 'video',
        src: 'sample-video.mp4',
        description: 'Sample Video'
    },
    'key014': {
        type: 'html',
        src: 'content1.html',
        description: 'Custom HTML Page'
    },
    'key015': {
        type: 'iframe',
        src: 'https://example.com',
        description: 'External Website'
    }
};
```

### Content Types

1. **Image** (`type: 'image'`)
   - Displays images with proper centering and scaling
   - Supports JPG, PNG, GIF, SVG, etc.
   - Example: `{ type: 'image', src: 'photo.jpg', description: 'My Photo' }`

2. **Video** (`type: 'video'`)
   - Plays videos with controls
   - Auto-plays (muted) and loops
   - Example: `{ type: 'video', src: 'video.mp4', description: 'My Video' }`

3. **HTML** (`type: 'html'`)
   - Loads custom HTML files from your project
   - Can include CSS, JavaScript, interactive elements
   - Example: `{ type: 'html', src: 'game.html', description: 'Mini Game' }`

4. **IFrame** (`type: 'iframe'`)
   - Loads external websites
   - Note: Some sites block iframe embedding
   - Example: `{ type: 'iframe', src: 'https://example.com', description: 'Website' }`

## File Structure

```
website/
├── index.html           # Main file with 3D scene
├── keyboard.glb         # 3D keyboard model
├── monitor.glb          # 3D monitor model
├── aste_logo.jpg        # Sample image
├── content1.html        # Example custom HTML page
├── game.html            # Example simple game
└── README.md            # This file
```

## Finding Key Names

1. Open `index.html` in a browser
2. Press F12 to open Developer Console
3. Click on any key
4. The console will show: `You clicked on: key012` (or similar)
5. Use this name in your `keyContentMap`

## WordPress Integration

### Method 1: Using Custom HTML Block (Recommended)

1. **Upload Files to WordPress Media Library:**
   - Upload `index.html`, `keyboard.glb`, `monitor.glb`, and content files
   - Note the URLs of uploaded files

2. **Create a New Page/Post:**
   - Add a "Custom HTML" block
   - Copy the entire contents of `index.html`
   - Update all file paths to use WordPress media URLs:
     ```javascript
     const modelUrl = 'https://yoursite.com/wp-content/uploads/2025/keyboard.glb';
     const monitorPlaceholderUrl = 'https://yoursite.com/wp-content/uploads/2025/monitor.glb';
     ```
   - Update image/video paths in `keyContentMap` similarly

3. **Set Full Width:**
   - Make the page full-width (depends on your theme)
   - Or set container height in the CSS: `#container { height: 600px; }`

### Method 2: Using a Plugin

1. **Install "Insert Headers and Footers" or similar plugin**
2. **Upload files via FTP or File Manager to:**
   - `/wp-content/uploads/keyboard-project/`
3. **Create an iframe in your page:**
   ```html
   <iframe src="/wp-content/uploads/keyboard-project/index.html" 
           style="width:100%; height:600px; border:none;"></iframe>
   ```

### Method 3: Custom Shortcode (Advanced)

Create a plugin or add to `functions.php`:

```php
function keyboard_3d_shortcode() {
    $upload_dir = wp_upload_dir();
    $base_url = $upload_dir['baseurl'] . '/keyboard-project/';
    
    ob_start();
    include($upload_dir['basedir'] . '/keyboard-project/index.html');
    return ob_get_clean();
}
add_shortcode('keyboard_3d', 'keyboard_3d_shortcode');
```

Then use `[keyboard_3d]` in your page.

## Important WordPress Considerations

1. **File Paths:** Use absolute URLs for all assets in WordPress
2. **CORS Issues:** Some external sites won't load in iframes due to CORS policies
3. **Security:** WordPress may strip certain JavaScript - use Custom HTML blocks or plugins that allow scripts
4. **Performance:** Consider using a CDN for 3D models if they're large
5. **Mobile:** Test on mobile devices - 3D performance varies

## Customization Tips

### Adjust Monitor Size
In the monitor loading section:
```javascript
monitorModel.scale.set(2, 2, 2); // Change these numbers
```

### Change Camera Position
```javascript
camera.position.set(0, 0.31, 0.25); // X, Y, Z coordinates
controls.target.set(0, 0.2, 0); // Where camera looks
```

### Add More Keys
Simply add more entries to `keyContentMap` with the appropriate key names.

### Style the Info Box
Edit the CSS in the `<style>` section:
```css
#info-box {
    /* Modify these styles */
    top: 20px;
    left: 20px;
    background-color: rgba(255, 255, 255, 0.9);
}
```

## Troubleshooting

**Q: The iframe doesn't match the monitor size**
A: The `updateIframePosition()` function now stretches the iframe to fill the entire monitor mesh. Content inside uses `object-fit: contain` to maintain aspect ratio.

**Q: My video won't play**
A: Make sure the video format is web-compatible (MP4 with H.264). Also check file paths.

**Q: External website won't load in iframe**
A: Many sites block iframe embedding for security. Try different sites or use screenshots instead.

**Q: Keys don't respond**
A: Check the browser console for key names. Make sure the key name matches exactly in `keyContentMap`.

**Q: WordPress strips my JavaScript**
A: Use a plugin like "Code Snippets" or "Insert Headers and Footers" that preserves JavaScript, or use the Custom HTML block in Gutenberg.

## Browser Compatibility

- ✅ Chrome/Edge (Recommended)
- ✅ Firefox
- ✅ Safari (may have minor WebGL differences)
- ⚠️ Mobile browsers (performance dependent on device)

## License
Free to use and modify for your projects!

## Credits
- Three.js for 3D rendering
- GSAP for animations
- Your 3D models (keyboard.glb, monitor.glb)
