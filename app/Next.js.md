Optimizing Fonts and Images


1. Why optimize fonts?

"FOnts" play a significant role in the design of a Website, but using "custom fonts" in your project can "affect performance" if the "font files" need to be "fetched and loaded".


=> "Cumulative Layout Shift" -> Is a "metric" used by "Google" to "evaluate" the "performance" and "user experience" of a website.


=> With "fonts", "layout shift" happens when the browser "initially renders" text in a "fallback or system font" and then swaps it out for a "custom font" once it has "loaded".

This "swap" can cause the "text size, spacing, or layout" to "Change", "shifting elements" around it.


=> Next.js automatically "optimizes fonts" in the application when you use the "next/font" module.

-> It downloads "font files" at "build time"
 and "hosts" them with your other static assests.

-> This means when a "user" visits your application, there are no "additional network requests" for fonts which would "impact performance"


The "<Image>" Component

The "<Image>" Component is an extension of the HTML <img> tag, and comes with automatic "image optimization", such as:

1. Preventing "layout shift" automatically when images are loading.

2."Resizing images" to avoid shipping "large images" to devices with a smaller viewport.

3. "Lazy loading images" by default(images load as they enter the viewport)

4. "Serving images" in "modern formats", like WebP and AVIF, when the browser supports it.


Adding the desktop hero image

Let's use the <Image> component. If you look inside the "/public" folder, you'll see there are two images: "hero-desktop.png" and "hero-mobile.png". 

These two images are completely different, and they'll be shown depending if the "user's device is a desktop or mobile.


Iny your "/app/page.tsx" file, import the component from "next/image".

import AcmeLogo from '@/app/ui/acme-logo';
import { ArrowRightIcon } from '@heroicons/react/24/outline';
import Link from 'next/link';
import { lusitana } from '@/app/ui/fonts';
import Image from 'next/image';
 
export default function Page() {
  return (
    // ...
    <div className="flex items-center justify-center p-6 md:w-3/5 md:px-28 md:py-12">
      {/* Add Hero Images Here */}
      <Image
        src="/hero-desktop.png"
        width={1000}
        height={760}
        className="hidden md:block"
        alt="Screenshots of the dashboard project showing desktop version"
      />
    </div>
    //...
  );
}

=> Here, you're setting the "width" to "1000" and "height" to "760" pixels.

It's good practice to set the "width" and "height" of your images to avoid "layout shift", these should be an "aspect ratio identical" to the source image.

=> These "values" are not the "size" the image is rendered, but instead the "size" of the "actual image file" used to understand the aspect ration.



Chapter 4

Creating Layouts and Pages

> Nested routing

Next.js uses "file-system routing" where "folders" are used to create "nested routes".

=> Each "folder" represents a "route segment" that maps to a "URL segment".

app/dashboard/invoices -> URL Path

app -> Root Segment

dashboard -> Segment

invoices -> Leaf Segment

=> YOu can create "separate UIs" for each route using "layout.tsx" and pages.tsx" files.


=> "page.tsx" is a "special Next.js file" that "exports a React component", and it's required for the "route to be accessible".

=> To create a "nested route", you can "nest folders" inside each other and add "page.tsx" files inside them.

For Example:

app/page.tsx/dashboard/pages.tsx

/app/dashboard/page.tsx is associated with the "/dashboard" path.


Creating the dashboard layout

Dashboards have some sort of navigation that is shared across multiple pages.

In Next.js, you can use a "special layout.tsx" file to create UI that is shared between multiple pages.

import SideNav from '@/app/ui/dashboard/sidenav';
 
export default function Layout({ children }: { children: React.ReactNode }) {
  return (
    <div className="flex h-screen flex-col md:flex-row md:overflow-hidden">
      <div className="w-full flex-none md:w-64">
        <SideNav />
      </div>
      <div className="grow p-6 md:overflow-y-auto md:p-12">{children}</div>
    </div>
  );
}

The "<Layout />" component receives a "children prop".

This child can either be a "page" or "another layout".

=> One benefits of using "layouts" in Next.js is that on "navigation", only the page components update while the layout won't re-render.

This is called "partial rendering" which preserves "client-side" React state in the layout when transitioning between pages.




Root Layout

import '@/app/ui/global.css';
import { inter } from '@/app/ui/fonts';
 
export default function RootLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <html lang="en">
      <body className={`${inter.className} antialiased`}>{children}</body>
    </html>
  );
}

This is called a "root layout" and is required in every Next.js application.

Any UI(User Interfaces) you add to the "root layout" will be shared across "all pages" in your application.

=> You can use the "root layout" to "modify" your "<html>" and "<body>" tags and add metadata.


Chapter 6

Why optimize navigation?

To "link between pages", you'd traditionally use the "<a>" HTML element.

At the moment, the "sidebar" links use <a> elements, but notice what happens when you navigate between the home,invoices, and customers pages on your browser.

There's a full page refresh on each page navigation.


The "<Link> Component

In Next.js, you can use the "<Link />" Component to "link" between pages in your application.

=> <Link> allows you to do "client-side navigation" with JavaScript.

To use the <Link /> component, open /app/ui/dashboard/nav-links.tsx, and import the Link component from next/link. Then, replace the <a> tag with <Link>:

import {
  UserGroupIcon,
  HomeIcon,
  DocumentDuplicateIcon,
} from '@heroicons/react/24/outline';
import Link from 'next/link';
 
// ...
 
export default function NavLinks() {
  return (
    <>
      {links.map((link) => {
        const LinkIcon = link.icon;
        return (
          <Link
            key={link.name}
            href={link.href}
            className="flex h-[48px] grow items-center justify-center gap-2 rounded-md bg-gray-50 p-3 text-sm font-medium hover:bg-sky-100 hover:text-blue-600 md:flex-none md:justify-start md:p-2 md:px-3"
          >
            <LinkIcon className="w-6" />
            <p className="hidden md:block">{link.name}</p>
          </Link>
        );
      })}
    </>
  );
}


> Automatic code-splitting and prefetching

To improve the "navigation experience", Next.js automatically "code splits" your application by "route segments".

=> This is different from a traditional React SPA, where the browser loads all your application code on the initial page load.


=> "Splitting code by routes" means that pages become "isolated".

-> If a certain page throws an error, the rest of the application will still work.

This is also less code for the browser to "parse", which makes your application faster.

-> Furthermore, in "production",whenever "<Link>" components appear in the browser's viewport, Next.js automatically "prefetches" the code for the "linked route" in the background.

By the time the user clicks the link, the code for the destination page will already be loaded in the background, and this is what makes the page transition near-instant.


> Pattern: Showing active links

A common UI pattern is to show an "active" link to indicate to the user what page they are currently on.

To do this, you need to get the user's "current path" from the "URL".

Next.js provides a hook called "usePathname()" that you can use to check the "path" and "implement this pattern"

=> Since "usePathname()" is a React hook, you'll need to turn "nav-links.tsx" into a "Client COmponent".

Add React's "use client" directive to the top of the file, then import "usePathname()" from "next/navigation":

'use client';
 
import {
  UserGroupIcon,
  HomeIcon,
  DocumentDuplicateIcon,
} from '@heroicons/react/24/outline';
import Link from 'next/link';
import { usePathname } from 'next/navigation';
 
// ...

Next, assign the "path" to a "variable" called "pathname" inside your "<NavLinks />" component:


export default function NavLinks() {
  const pathname = usePathname();
  // ...
}







