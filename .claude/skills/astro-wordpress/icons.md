# Icons Guide (Lucide)

Icons are pre-installed via the lucide-astro package.

## Basic Usage
```astro
---
import { ArrowRight, Mail, Phone } from 'lucide-astro';
---

<ArrowRight class="w-5 h-5" />
<Mail class="w-6 h-6 text-primary" />
<Phone class="w-4 h-4 text-content-light" />
```

## Common Icons
- Navigation: ArrowRight, ArrowLeft, ChevronDown, ChevronRight, Menu, X
- Actions: Plus, Minus, Edit, Trash, Save, Download, Upload, Share
- Communication: Mail, Phone, MessageCircle, Send
- Social: Github, Twitter, Facebook, Instagram, Linkedin
- Status: Check, CheckCircle, AlertCircle, AlertTriangle, Info
- UI: Search, Settings, User, Home, Star, Heart

## With Buttons
```astro
---
import { ArrowRight } from 'lucide-astro';
---

<button class="bg-primary text-white px-4 py-2 rounded-lg flex items-center gap-2 cursor-pointer">
    Learn More
    <ArrowRight class="w-4 h-4" />
</button>
```

## Dynamic Icons
```astro
---
import * as Icons from 'lucide-astro';
const iconName = 'ArrowRight';
const IconComponent = Icons[iconName];
---

<IconComponent class="w-5 h-5" />
```
