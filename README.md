# Novah Studio - Modern Hero Component

## Overview

**Novah Studio** is a sophisticated, full-screen hero component designed for creative agencies, design studios, and portfolio websites. Featuring a cinematic video background with an elegant grid-based layout, this component creates an immersive first impression while maintaining optimal readability and user engagement.

## Live Demo

[View Novah](https://thisislefa.github.io/Novah/)

## Technical Architecture

### Core Technologies

- **Pure HTML5 & CSS3** - No external dependencies or frameworks required
- **CSS Grid Layout** - Advanced 2x2 grid system for precise content placement
- **Responsive Video Background** - Full-screen video with overlay gradient
- **CSS Custom Properties** - Theming system with namespace isolation
- **Inter Tight Typeface** - Modern, condensed font for editorial aesthetic

### Design System

#### Typography
- **Primary Font:** Inter Tight (300-800 weights, condensed variant)
- **Main Title:** 16vw responsive sizing with precise letter-spacing
- **Subtitle:** 5vw with negative margins for visual alignment
- **Body Text:** 1.25rem with optimal line-height for readability

#### Color Palette
```css
--nv-white: #ffffff;          /* Primary text color */
--nv-black: #000000;          /* Button and card text */
--nv-card-bg: #ffffff;        /* Card background */
```

#### Layout Principles
- **2x2 Grid System:** Four distinct content zones with semantic placement
- **Viewport Units:** Responsive sizing using vw/vh for fluid adaptation
- **Z-index Stack:** Three-layer system (video → overlay → content)
- **Negative Margins:** Strategic alignment adjustments for visual balance

## Component Breakdown

### Video Background System
```css
.nv-video-bg {
    object-fit: cover;          /* Fill container while maintaining aspect */
    opacity: 0.7;               /* Subtle transparency for text contrast */
}

.nv-overlay {
    background: linear-gradient(135deg, 
        rgba(10, 5, 40, 0.562) 0%, 
        rgba(20, 10, 60, 0.244) 100%);
}
```

### Grid Layout Structure
```
Grid Areas:
┌─────────────┬─────────────┐
│ hero        │ services    │
│ (top-left)  │ (top-right) │
├─────────────┼─────────────┤
│ details     │ card        │
│ (bottom-left│ (bottom-right)
└─────────────┴─────────────┘
```

### Team Card Component
- **Two-column flex layout** (image + content)
- **Custom SVG arrow** for button interaction
- **White background with shadow** for contrast against dark video
- **Hover transformations** for interactive feedback

## Integration Guide

### Basic Implementation

1. **Include Dependencies:**
```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Inter+Tight:wght@300;400;500;600;700;800&display=swap" rel="stylesheet">
<link rel="stylesheet" href="novah.css">
```

2. **HTML Structure:**
```html
<div class="nv-wrapper">
    <video class="nv-video-bg" autoplay muted loop playsinline>
        <source src="your-background-video.mp4" type="video/mp4">
    </video>
    <div class="nv-overlay"></div>
    
    <div class="nv-container">
        <div class="nv-hero-text">
            <h1 class="nv-title-main">Your Brand</h1>
            <h2 class="nv-title-sub">Studio</h2>
        </div>
        
        <div class="nv-services">
            <!-- Service items -->
        </div>
        
        <div class="nv-description">
            <p class="nv-desc-text">Your description here.</p>
        </div>
        
        <div class="nv-bottom-right">
            <!-- Team card and copyright -->
        </div>
    </div>
</div>
```

### Video Optimization Guidelines

**Recommended Video Specifications:**
- **Format:** MP4 with H.264 encoding
- **Resolution:** 1920x1080 (1080p) minimum
- **Duration:** 15-30 seconds for seamless looping
- **File Size:** 5-10MB maximum for fast loading
- **Content:** Abstract, slow-moving patterns without text

**Fallback Strategies:**
```html
<video class="nv-video-bg" autoplay muted loop playsinline 
        poster="fallback-image.jpg">
    <source src="video.mp4" type="video/mp4">
    <source src="video.webm" type="video/webm">
    <!-- Fallback to gradient background -->
    <div class="nv-static-bg"></div>
</video>
```

## Advanced Integration Patterns

### React Component Implementation

```jsx
import React, { useRef, useEffect } from 'react';
import './NovahHero.css';

const NovahHero = ({ 
    title = "Novah", 
    subtitle = "Studio",
    services = [],
    description,
    teamMember,
    videoSrc 
}) => {
    const videoRef = useRef(null);
    
    useEffect(() => {
        // Lazy load video for performance
        if (videoRef.current) {
            videoRef.current.load();
        }
    }, [videoSrc]);
    
    return (
        <div className="nv-wrapper">
            <video 
                ref={videoRef}
                className="nv-video-bg" 
                autoPlay 
                muted 
                loop 
                playsInline
                preload="metadata"
            >
                <source src={videoSrc} type="video/mp4" />
            </video>
            <div className="nv-overlay"></div>
            
            <div className="nv-container">
                {/* Rest of component structure */}
            </div>
        </div>
    );
};

export default NovahHero;
```

### WordPress Integration

```php
<?php
// functions.php - Register custom fields
if (function_exists('acf_add_local_field_group')) {
    acf_add_local_field_group(array(
        'key' => 'group_novah_hero',
        'title' => 'Novah Hero Settings',
        'fields' => array(
            array(
                'key' => 'field_hero_video',
                'label' => 'Background Video',
                'name' => 'hero_video',
                'type' => 'file',
                'mime_types' => 'mp4',
            ),
            // Additional fields...
        ),
    ));
}

// template-parts/hero-novah.php
$video_url = get_field('hero_video');
$services = get_field('hero_services');
?>
<div class="nv-wrapper">
    <?php if ($video_url): ?>
    <video class="nv-video-bg" autoplay muted loop playsinline>
        <source src="<?php echo esc_url($video_url); ?>" type="video/mp4">
    </video>
    <?php endif; ?>
    <div class="nv-overlay"></div>
    
    <div class="nv-container">
        <!-- Dynamic content from ACF -->
    </div>
</div>
```

### Next.js with TypeScript

```typescript
// components/NovahHero.tsx
import { FC } from 'react';
import styles from './NovahHero.module.css';

interface ServiceItem {
  id: number;
  text: string;
}

interface TeamMember {
  name: string;
  role: string;
  image: string;
}

interface NovahHeroProps {
  title: string;
  subtitle: string;
  services: ServiceItem[];
  description: string;
  teamMember: TeamMember;
  videoSrc: string;
}

const NovahHero: FC<NovahHeroProps> = ({
  title,
  subtitle,
  services,
  description,
  teamMember,
  videoSrc
}) => {
  return (
    <div className={styles.wrapper}>
      <video 
        className={styles.videoBg}
        autoPlay
        muted
        loop
        playsInline
        preload="metadata"
      >
        <source src={videoSrc} type="video/mp4" />
      </video>
      <div className={styles.overlay} />
      
      <div className={styles.container}>
        {/* Grid implementation with TypeScript */}
      </div>
    </div>
  );
};

export default NovahHero;
```

## Performance Optimization

### Video Loading Strategies

**Lazy Loading Implementation:**
```javascript
// Intersection Observer for video loading
const videoObserver = new IntersectionObserver((entries) => {
    entries.forEach(entry => {
        if (entry.isIntersecting) {
            const video = entry.target;
            video.src = video.dataset.src;
            video.load();
            videoObserver.unobserve(video);
        }
    });
}, { threshold: 0.1 });

document.querySelectorAll('.nv-video-bg[data-src]').forEach(video => {
    videoObserver.observe(video);
});
```

**HTML Implementation:**
```html
<video class="nv-video-bg" 
       autoplay 
       muted 
       loop 
       playsinline
       data-src="video.mp4"
       poster="low-res-preview.jpg">
</video>
```

### CSS Optimization

**Critical CSS Inlining:**
```html
<style>
/* Critical above-the-fold styles */
.nv-wrapper, .nv-overlay, .nv-title-main, .nv-title-sub {
    /* Minimal styles for immediate rendering */
}
</style>

<link rel="stylesheet" href="novah.css" media="print" onload="this.media='all'">
```

## Customization Options

### Theming Configuration

```css
/* Custom theme overrides */
:root {
    --nv-font: 'Your Custom Font', sans-serif;
    --nv-primary-color: #your-color;
    --nv-gradient-overlay: linear-gradient(135deg, 
        rgba(your, color, values) 0%, 
        rgba(your, color, values) 100%);
}

/* Alternative color schemes */
.nv-wrapper.dark-theme {
    --nv-white: #f0f0f0;
    --nv-black: #1a1a1a;
}

.nv-wrapper.light-theme {
    background-color: #ffffff;
    color: #000000;
}

.nv-wrapper.light-theme .nv-overlay {
    background: linear-gradient(135deg, 
        rgba(255, 255, 255, 0.9) 0%, 
        rgba(255, 255, 255, 0.7) 100%);
}
```

### Layout Modifications

**Alternative Grid Configurations:**
```css
/* 3-column layout */
.nv-container.three-column {
    grid-template-columns: 1fr 1fr 1fr;
    grid-template-areas: 
        "hero hero services"
        "details card card";
}

/* Centered layout */
.nv-container.centered {
    grid-template-columns: 1fr;
    grid-template-areas: 
        "hero"
        "services"
        "details"
        "card";
    text-align: center;
}
```

## Browser Compatibility & Fallbacks

### Supported Features
- **CSS Grid:** Full support in modern browsers
- **Video Background:** MP4 with autoplay (muted) support
- **CSS Variables:** Native support in modern browsers
- **Object-fit:** Image/video cropping and positioning

### Fallback Strategies

**No Video Support:**
```css
.nv-wrapper.no-video .nv-video-bg {
    display: none;
}

.nv-wrapper.no-video {
    background: 
        linear-gradient(135deg, #0a0528 0%, #140a3c 100%),
        url('static-background.jpg') center/cover;
}
```

**IE11 Compatibility Layer:**
```css
/* Flexbox fallback for IE11 */
@media all and (-ms-high-contrast: none) {
    .nv-container {
        display: -ms-flexbox;
        -ms-flex-wrap: wrap;
    }
    
    .nv-hero-text, 
    .nv-services, 
    .nv-description, 
    .nv-bottom-right {
        -ms-flex: 1 1 50%;
    }
}
```

## Accessibility Considerations

### ARIA Implementation
```html
<div class="nv-wrapper" role="banner" aria-label="Hero section">
    <video class="nv-video-bg" 
           aria-label="Abstract background video"
           aria-describedby="video-description">
    </video>
    <div id="video-description" class="sr-only">
        Abstract video showing flowing particles and light effects
    </div>
    
    <div class="nv-container">
        <div class="nv-hero-text" aria-labelledby="main-title">
            <h1 id="main-title" class="nv-title-main">Novah</h1>
            <h2 class="nv-title-sub">Studio</h2>
        </div>
    </div>
</div>
```

### Screen Reader Optimizations
```css
/* Hide decorative video from screen readers */
.nv-video-bg[aria-hidden="true"] {
    display: none;
}

/* Focus styles for interactive elements */
.nv-card-btn:focus {
    outline: 2px solid #4d90fe;
    outline-offset: 2px;
}
```

## Performance Metrics

### Target Load Times
- **First Contentful Paint:** < 1.5 seconds
- **Largest Contentful Paint:** < 2.5 seconds
- **Cumulative Layout Shift:** < 0.1
- **Total Blocking Time:** < 200ms

### Optimization Checklist
1. [ ] Video compressed to < 5MB
2. [ ] Critical CSS inlined
3. [ ] Fonts preloaded with `font-display: swap`
4. [ ] Images optimized with WebP format
5. [ ] Lazy loading implemented for non-critical resources
6. [ ] Service worker caching for repeat visits

## Development Workflow

### Build Tools Integration

**Webpack Configuration:**
```javascript
// webpack.config.js
module.exports = {
    module: {
        rules: [
            {
                test: /\.css$/,
                use: [
                    'style-loader',
                    {
                        loader: 'css-loader',
                        options: {
                            importLoaders: 1,
                        }
                    },
                    'postcss-loader'
                ]
            },
            {
                test: /\.(mp4|webm)$/,
                use: [
                    {
                        loader: 'file-loader',
                        options: {
                            name: '[name].[ext]',
                            outputPath: 'videos/'
                        }
                    }
                ]
            }
        ]
    }
};
```

**PostCSS Configuration:**
```javascript
// postcss.config.js
module.exports = {
    plugins: [
        require('autoprefixer'),
        require('cssnano')({
            preset: 'default',
        }),
    ]
};
```

## Deployment Guidelines

### Static Hosting
```bash
# Build and deploy to GitHub Pages
npm run build
git add dist/
git commit -m "Deploy Novah component"
git subtree push --prefix dist origin gh-pages
```

### CDN Configuration
```html
<!-- CDN-hosted version -->
<link rel="stylesheet" 
      href="https://cdn.yourdomain.com/novah/1.0.0/novah.min.css">

<script type="module">
    import NovahHero from 'https://cdn.yourdomain.com/novah/1.0.0/novah.esm.js';
</script>
```

## Contribution Guidelines

### Development Setup
```bash
# Clone repository
git clone https://github.com/thisislefa/novah-hero.git
cd novah-hero

# Install dependencies
npm install

# Start development server
npm run dev

# Build for production
npm run build
```

### Code Standards
- Follow BEM naming convention with `nv-` prefix
- Maintain mobile-first responsive approach
- Include accessibility features in all components
- Document complex CSS with comments
- Test in multiple browsers before submission

### Testing Checklist
- [ ] Video loads and plays correctly
- [ ] Grid layout adapts to all screen sizes
- [ ] Text remains readable at all zoom levels
- [ ] Interactive elements have proper focus states
- [ ] Performance metrics meet targets
- [ ] Accessibility audit passes WCAG 2.1 AA

## License & Usage

This component is available for personal and commercial use under the MIT License. Attribution is appreciated but not required.

## Support & Resources

- **Documentation:** [GitHub Wiki](https://github.com/thisislefa/novah-hero/wiki)
- **Issue Tracker:** [GitHub Issues](https://github.com/thisislefa/novah-hero/issues)
- **Examples:** See `/examples` directory for implementation patterns
- **Community:** Share your implementations with #NovahHero

---

*Novah Studio represents a production-ready hero component that balances visual impact with technical robustness. Its modular architecture and comprehensive documentation make it suitable for integration into projects ranging from simple portfolio sites to complex enterprise applications.*
