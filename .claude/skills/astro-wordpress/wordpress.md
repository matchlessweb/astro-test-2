# WordPress Integration Guide

## Posts
```typescript
const posts = await getPosts({ page: 1, perPage: 10 });
const post = await getPost('my-post-slug');

// Render content safely
<article set:html={post.content.rendered} />
```

## Pages
```typescript
const pages = await getPages();
const page = await getPage('about');
```

## Featured Images
```typescript
const imageUrl = getFeaturedImageUrl(post, 'large');
// Sizes: thumbnail, medium, large, full

{post._embedded?.['wp:featuredmedia']?.[0] && (
    <img src={getFeaturedImageUrl(post, 'large')} alt={post.title.rendered} />
)}
```

## Custom Requests
```typescript
// For custom post types, ACF, etc.
const data = await makeCMSRequest('/wp/v2/custom-type');
const acfData = await makeCMSRequest('/acf/v3/posts');
```

## Forms (Fluent Forms)
```typescript
await submitFluentForm(formId, {
    name: 'John',
    email: 'john@example.com',
    message: 'Hello!'
});
```

## Comments
```typescript
await createComment(postId, 'Great post!', {
    name: 'Commenter',
    email: 'commenter@example.com'
});
```

## Navigation
```typescript
const navPages = await getNavigationPages();
const menuItems = await getMenuItems('main');
const isActive = isActivePath(Astro.url.pathname, '/about');
```
