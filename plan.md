Below is a detailed, step-by-step implementation plan using proper markdown. This plan covers all dependent files, error handling, best practices, and realistic feature sets for a modern, responsive website for the Behrouz Elite Swimming Team.

---

## Overview

The goal is to build a Next.js localhost site for the Behrouz Elite Swimming Team that features:  
- A modern landing page with a hero banner, description, and clearly laid-out branch information.  
- A header component with navigation links (Home, About, Branches, Contact).  
- Branch cards displaying details for the Parañaque City and Lucena City branches (with a placeholder for expansion).  

All UI elements are built with careful use of typography, spacing, colors, and layout. No external icon or image libraries are used except for explicit landing page images (created with a placeholder service). Error handling (such as onError fallbacks for images) and semantic HTML structure (accessible navigation, ARIA attributes) are incorporated.

---

## File Changes and New Files

### 1. Create/Update Layout File: `src/app/layout.tsx`

- **Purpose**: Provide the global page structure and metadata.  
- **Steps**:
  - Create a new file `src/app/layout.tsx`.
  - Import `globals.css` from `src/app/globals.css`.
  - Set up an HTML document wrapper with `<html>`, `<head>`, and `<body>`.
  - Optionally import the header component here to act as a site-wide navigation bar.
  - Return `{children}` so that every page (e.g., the home page) is nested within the layout.

**Example snippet:**
```typescript
// src/app/layout.tsx
import '../app/globals.css'
import TeamHeader from '../components/TeamHeader'

export default function RootLayout({ children }: { children: React.ReactNode }) {
  return (
    <html lang="en">
      <head>
        <title>Behrouz Elite Swimming Team</title>
        <meta name="description" content="Official website for Behrouz Elite Swimming Team branches." />
      </head>
      <body>
        <TeamHeader />
        {children}
      </body>
    </html>
  )
}
```

---

### 2. Create Home Page: `src/app/page.tsx`

- **Purpose**: Serve as the landing page with the hero section and branch details.  
- **Steps**:
  - Create a new file `src/app/page.tsx`.
  - Import the branch card component from `src/components/BranchCard`.
  - Add a responsive hero section with:
    - A headline with the team name.
    - A short description.
    - An image banner using a placeholder URL (with onError fallback).
  - Below the hero, display branch cards for both "Parañaque City" and "Lucena City" using the BranchCard component.
  - Use semantic HTML elements like `<main>`, `<section>`, and `<article>`.

**Example snippet:**
```typescript
// src/app/page.tsx
import BranchCard from '../components/BranchCard'

export default function HomePage() {
  return (
    <main className="container mx-auto p-6">
      <section className="hero mb-10 text-center">
        <h1 className="text-4xl font-bold mb-4">Behrouz Elite Swimming Team</h1>
        <p className="text-lg mb-6">Excellence in aquatic sports with branches in Parañaque City and Lucena City – expanding soon!</p>
        <img 
          src="https://placehold.co/1920x1080?text=Modern+team+leader+banner+for+Behrouz+Elite+Swimming+Team" 
          alt="Modern team leader banner for Behrouz Elite Swimming Team" 
          onError={(e) => { e.currentTarget.onerror = null; e.currentTarget.src = '/fallback-image.png'; }} 
          className="w-full h-auto object-cover rounded"
        />
      </section>
      
      <section className="branches grid gap-6 md:grid-cols-2">
        <BranchCard 
          branchName="Parañaque City" 
          description="Our state-of-the-art facility in Parañaque City offers top-notch training and excellent coaching staff." 
        />
        <BranchCard 
          branchName="Lucena City" 
          description="Located in the heart of Lucena City, this branch is geared to nurture emerging talent in swimming techniques." 
        />
      </section>
    </main>
  )
}
```

---

### 3. Create Header Component: `src/components/TeamHeader.tsx`

- **Purpose**: Provide a modern, accessible navigation bar for site-wide linking.  
- **Steps**:
  - Create a new file `src/components/TeamHeader.tsx`.
  - Use a `<header>` element with a `<nav>` section.
  - Include the team name as a logo/text element (styled with modern typography).
  - Build a navigation menu with links to Home, About, Branches, and Contact.
  - Use semantic HTML (`<ul>`/`<li>`) with aria labels where needed.

**Example snippet:**
```typescript
// src/components/TeamHeader.tsx
import Link from 'next/link'

export default function TeamHeader() {
  return (
    <header className="bg-blue-600 text-white p-4">
      <div className="container mx-auto flex justify-between items-center">
        <h2 className="text-xl font-semibold">Behrouz Elite Swimming Team</h2>
        <nav>
          <ul className="flex space-x-4">
            <li><Link href="/"><a aria-label="Home page">Home</a></Link></li>
            <li><Link href="/about"><a aria-label="About team">About</a></Link></li>
            <li><Link href="/branches"><a aria-label="Team branch locations">Branches</a></Link></li>
            <li><Link href="/contact"><a aria-label="Contact team">Contact</a></Link></li>
          </ul>
        </nav>
      </div>
    </header>
  )
}
```

---

### 4. Create Branch Card Component: `src/components/BranchCard.tsx`

- **Purpose**: Reusable component to display branch details.  
- **Steps**:
  - Create a new file `src/components/BranchCard.tsx`.
  - Define TypeScript props (branchName and description).
  - Use the prebuilt Card component from `src/components/ui/card.tsx` (if needed), or implement a simple card layout.
  - Display the branch information with a title and descriptive text.
  - (Optional) Add a placeholder image using the `<img>` tag with a fallback handler.

**Example snippet:**
```typescript
// src/components/BranchCard.tsx
type BranchCardProps = {
  branchName: string;
  description: string;
}

export default function BranchCard({ branchName, description }: BranchCardProps) {
  return (
    <article className="border rounded-lg p-6 shadow hover:shadow-lg transition duration-300">
      <h3 className="text-2xl font-bold mb-3">{branchName}</h3>
      <p className="text-base mb-4">{description}</p>
      <img 
        src={`https://placehold.co/600x400?text=${encodeURIComponent(branchName + '+branch+display')}`} 
        alt={`Placeholder image showing ${branchName} branch facility with modern design and training environment`} 
        onError={(e) => { e.currentTarget.onerror = null; e.currentTarget.src = '/fallback-branch.png'; }} 
        className="w-full h-auto rounded"
      />
    </article>
  )
}
```

---

### 5. Update Global Styles: `src/app/globals.css`

- **Purpose**: Ensure consistency and modern look across all elements.  
- **Steps**:
  - Open `src/app/globals.css`.
  - Add or modify styles to define a modern color palette (e.g., blue accents, neutral backgrounds) and typography.
  - Include responsive grid/container settings.
  - Optionally add custom classes for hero, branch cards, header, and footer.

**Example additions:**
```css
/* src/app/globals.css */

/* Basic Reset and Typography */
body {
  font-family: "Helvetica Neue", Helvetica, Arial, sans-serif;
  line-height: 1.6;
  background-color: #f9f9f9;
  margin: 0;
  padding: 0;
}

/* Container */
.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 1rem;
}

/* Hero Section */
.hero {
  background-color: #e0f7fa;
  padding: 2rem;
}

/* Header Styling */
header {
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}

/* Responsive grid for branches */
.branches {
  display: grid;
  gap: 1.5rem;
}
```

---

### 6. Update README.md

- **Purpose**: Provide instructions to run and develop the site.  
- **Steps**:
  - Open `README.md`.
  - Add a section on how to install dependencies and start the development server on localhost.
  - Include project details and a brief description of the website features.

**Example snippet:**
```markdown
# Behrouz Elite Swimming Team Website

This project is a Next.js site showcasing the Behrouz Elite Swimming Team with branches in Parañaque City and Lucena City, and plans for future expansion.

## Getting Started

1. Install dependencies:
   ```
   npm install
   ```
2. Run the development server:
   ```
   npm run dev
   ```
3. Open [http://localhost:3000](http://localhost:3000) in your browser.
```

---

## Error Handling & Best Practices

- **Images**: All `<img>` tags include an `onError` handler to set a fallback image source.
- **Accessibility**: Use semantic HTML (`<header>`, `<nav>`, `<main>`, `<article>`) along with proper aria-labels.
- **Type Safety**: Use TypeScript for props definitions in components such as BranchCard.
- **Responsive Design**: Implement container and grid settings in `globals.css` to ensure a modern, responsive UI.
- **Component Reuse**: Utilize existing UI elements from `src/components/ui/` where applicable (such as the Card component).

---

## Summary

- Created a global layout in `src/app/layout.tsx` that wraps all pages with header and metadata.  
- Developed a modern home page in `src/app/page.tsx` featuring a hero section and branch cards.  
- Implemented a reusable header (`TeamHeader.tsx`) for accessible, semantic navigation.  
- Built a Branch Card component (`BranchCard.tsx`) to display branch-specific details, using fallback handling for images.  
- Updated `src/app/globals.css` for responsive, contemporary styles.  
- Revised `README.md` with instructions for local development and installation.  
This plan ensures a modern UI/UX with error handling, semantic HTML, and scalable design for future expansion.
