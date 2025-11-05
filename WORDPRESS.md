# WordPress Integration Guide

## Quick Start for WordPress

### Option 1: Embed as Full Page (Easiest)

1. **Upload all files to WordPress Media Library:**
   - Go to Media → Add New
   - Upload: `index.html`, `keyboard.glb`, `monitor.glb`, `aste_logo.jpg`
   - Also upload: `content1.html`, `game.html`, `gallery.html`
   - Copy the URL of each uploaded file

2. **Create a new Page:**
   - Add a "Custom HTML" block
   - Paste the ENTIRE contents of `index.html`

3. **Update File Paths:**
   Replace these lines in the pasted HTML:
   ```javascript
   // Change this:
   const modelUrl = 'keyboard.glb';
   // To this:
   const modelUrl = 'https://yoursite.com/wp-content/uploads/2025/11/keyboard.glb';

   // Change this:
   const monitorPlaceholderUrl = 'monitor.glb';
   // To this:
   const monitorPlaceholderUrl = 'https://yoursite.com/wp-content/uploads/2025/11/monitor.glb';

   // In keyContentMap, update all src paths:
   'key012': {
       type: 'image',
       src: 'https://yoursite.com/wp-content/uploads/2025/11/aste_logo.jpg',
       description: 'ASTE Logo'
   },
   'key014': {
       type: 'html',
       src: 'https://yoursite.com/wp-content/uploads/2025/11/content1.html',
       description: 'Custom HTML Page'
   },
   // etc...
   ```

4. **Set Page Template:**
   - In page settings, choose "Full Width" template
   - Or add this CSS to make it taller:
   ```css
   #container {
       height: 80vh; /* or 600px */
   }
   ```

5. **Publish!**

---

### Option 2: Embed in Any Page/Post

If you want to embed this in a specific section of a page:

1. **Upload files via FTP to a folder:**
   - Create: `/wp-content/uploads/keyboard-3d/`
   - Upload all your files there

2. **In your page, add Custom HTML block:**
   ```html
   <div style="width: 100%; height: 600px; position: relative;">
       <iframe 
           src="/wp-content/uploads/keyboard-3d/index.html" 
           style="width: 100%; height: 100%; border: none;"
           allowfullscreen>
       </iframe>
   </div>
   ```

---

### Option 3: Using Shortcode (For Multiple Uses)

Add this to your theme's `functions.php` or create a custom plugin:

```php
<?php
/**
 * Plugin Name: 3D Keyboard Shortcode
 * Description: Embed interactive 3D keyboard anywhere with [keyboard_3d]
 * Version: 1.0
 */

function keyboard_3d_shortcode($atts) {
    // Parse attributes
    $atts = shortcode_atts(array(
        'height' => '600px',
        'width' => '100%'
    ), $atts);
    
    // Get upload directory
    $upload_dir = wp_upload_dir();
    $keyboard_url = $upload_dir['baseurl'] . '/keyboard-3d/index.html';
    
    // Return iframe
    return sprintf(
        '<div style="width: %s; height: %s; position: relative;">
            <iframe 
                src="%s" 
                style="width: 100%%; height: 100%%; border: none;"
                allowfullscreen>
            </iframe>
        </div>',
        esc_attr($atts['width']),
        esc_attr($atts['height']),
        esc_url($keyboard_url)
    );
}
add_shortcode('keyboard_3d', 'keyboard_3d_shortcode');
```

**Usage:**
```
[keyboard_3d]
[keyboard_3d height="800px"]
[keyboard_3d width="90%" height="700px"]
```

---

## Important WordPress Considerations

### 1. Security & Script Blocking
WordPress may strip JavaScript from Custom HTML blocks. Solutions:
- Use a plugin like **"Insert Headers and Footers"**
- Or **"Code Snippets"** 
- Or **"WPCode"** (allows HTML/JS/CSS)
- Make sure your user role has `unfiltered_html` capability

### 2. CORS & External Content
If loading external websites in iframes:
- Many sites (Google, Facebook, etc.) block iframe embedding
- Check browser console for "X-Frame-Options" errors
- Use screenshots as alternatives for blocked sites

### 3. Performance
- 3D models can be large - optimize GLB files
- Consider using a CDN for assets
- Test on mobile devices - performance varies
- Lazy load the 3D scene if below the fold

### 4. Mobile Optimization
Add this meta tag if not already present:
```html
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```

For better mobile experience, you might want to:
```javascript
// Detect mobile and adjust camera
if (window.innerWidth < 768) {
    camera.position.set(0, 0.5, 0.4); // Further back on mobile
}
```

### 5. WordPress Theme Compatibility
Some themes inject CSS that might interfere:
```css
/* Add this to your Custom CSS or theme's style.css */
#container canvas {
    display: block !important;
    width: 100% !important;
    height: 100% !important;
}

#monitor-iframe {
    pointer-events: auto !important;
}
```

---

## Testing Checklist

Before going live on WordPress:

- [ ] All 3D models load correctly
- [ ] All keys respond to clicks
- [ ] Images display properly
- [ ] Videos play (with controls)
- [ ] Custom HTML pages load
- [ ] Monitor turns on/off as expected
- [ ] Responsive on tablet
- [ ] Works on mobile (at least displays)
- [ ] Browser console shows no errors
- [ ] All file paths are absolute URLs

---

## Troubleshooting WordPress Issues

**Problem:** "Failed to load model"
- Solution: Check that GLB files are uploaded and URLs are correct
- Make sure file extensions are allowed in WordPress

**Problem:** JavaScript doesn't execute
- Solution: Use a plugin that allows scripts, or check user permissions

**Problem:** Iframe is blank
- Solution: Check browser console for CORS errors
- Verify the iframe src URL is correct and accessible

**Problem:** Content doesn't fit the monitor
- Solution: The iframe now stretches to fill monitor mesh
- Content inside uses `object-fit: contain` for proper aspect ratio

**Problem:** Page loads slowly
- Solution: Optimize GLB files using tools like glTF-Transform
- Consider using a CDN for larger assets

---

## Advanced: Dynamic Content from WordPress

To load WordPress posts/pages as monitor content:

```php
// In functions.php
function get_keyboard_content() {
    $posts = get_posts(array('numberposts' => 5));
    $content_map = array();
    
    $key_num = 12;
    foreach ($posts as $post) {
        $content_map['key0' . $key_num] = array(
            'type' => 'iframe',
            'src' => get_permalink($post->ID),
            'description' => $post->post_title
        );
        $key_num++;
    }
    
    return json_encode($content_map);
}

// Then in your HTML:
<script>
    const dynamicContent = <?php echo get_keyboard_content(); ?>;
    // Merge with or replace keyContentMap
    Object.assign(keyContentMap, dynamicContent);
</script>
```

---

## Need Help?

Check the main README.md for detailed customization options and troubleshooting.
