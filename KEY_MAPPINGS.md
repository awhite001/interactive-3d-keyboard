# Keyboard Key Mappings

## Current Key Assignments

| Key | Type | Content | Description |
|-----|------|---------|-------------|
| **key000** | HTML | content1.html | Custom HTML Page |
| **key001** | HTML | gallery.html | Interactive Gallery |
| **key002** | HTML | game.html | Mini Game (Dodge) |
| **key003** | YouTube | Rick Astley | Music Video |
| **key004** | Image | aste_logo.jpg | Company Logo |
| **key005** | iframe | example.com | Example Website |
| **key006** | YouTube | Me at the zoo | First YouTube Video |
| **key007** | HTML | content1.html | Info Page |
| **key008** | Image | aste_logo.jpg | Logo Alt |
| **key009** | HTML | gallery.html | Photo Gallery |
| **key010** | HTML | game.html | Play Game |
| **key011** | YouTube | Gangnam Style | Popular Video |
| **key012** | Image | aste_logo.jpg | ASTE Logo |
| **key013** | YouTube | Sample Video | YouTube Demo |
| **key014** | HTML | content1.html | Custom HTML Page |
| **key015** | iframe | example.com | External Website |

## How to Add More Keys

1. Open `index.html`
2. Find the `keyContentMap` object (around line 110)
3. Add a new entry:

```javascript
'key016': {
    type: 'image',        // or 'html', 'youtube', 'iframe'
    src: 'your-file.jpg', // path to your content
    description: 'My Image' // description shown in UI
},
```

## Finding Key Names

1. Open the page in a browser
2. Press F12 to open console
3. Click any key on the keyboard
4. Console will show: `You clicked on: keyXXX`
5. Use that name in `keyContentMap`

## Content Types

### 1. Image
```javascript
type: 'image',
src: 'path/to/image.jpg',
```
Works with: JPG, PNG, GIF, SVG
✅ Renders as texture on monitor
✅ Rotates with 3D view

### 2. HTML
```javascript
type: 'html',
src: 'path/to/page.html',
```
Loads local HTML files
⚠️ Shows placeholder due to CORS
💡 Best for custom pages

### 3. YouTube
```javascript
type: 'youtube',
src: 'https://www.youtube.com/embed/VIDEO_ID',
```
Use `/embed/` URL format
⚠️ Shows placeholder (can't capture iframe)
💡 For demonstration purposes

### 4. iframe
```javascript
type: 'iframe',
src: 'https://example.com',
```
Load external websites
⚠️ Shows placeholder due to CORS
💡 Many sites block iframe embedding

## Note on Image Rotation

If images appear rotated or flipped:
1. Check UV mapping in Blender
2. Adjust the monitor mesh's UV coordinates
3. Ensure the screen face is mapped correctly

Current behavior: Images render as textures on the 3D mesh, so they follow the monitor's orientation and UV mapping.

## Customization Tips

### Change Image for Multiple Keys
```javascript
'key004': { type: 'image', src: 'logo1.jpg', description: 'Logo 1' },
'key008': { type: 'image', src: 'logo2.jpg', description: 'Logo 2' },
'key012': { type: 'image', src: 'logo3.jpg', description: 'Logo 3' },
```

### Create Content Collections
Group similar content:
- Keys 000-003: Info/Documentation
- Keys 004-007: Media Gallery
- Keys 008-011: Games/Interactive
- Keys 012-015: External Links

### Add Your Own HTML Pages
Create new HTML files in the same directory:
```javascript
'key016': { type: 'html', src: 'about.html', description: 'About Us' },
'key017': { type: 'html', src: 'services.html', description: 'Our Services' },
```

## Toggle Feature

Click any assigned key twice:
1. First click: Display content
2. Second click: Turn monitor off (black screen)

This works automatically for all keys!
