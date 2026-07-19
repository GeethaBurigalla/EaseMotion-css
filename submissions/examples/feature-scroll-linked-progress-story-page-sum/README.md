# Scroll-Linked Progress Story Page

A long-form article demo that orchestrates three coordinated motions as
the reader scrolls: a top progress bar, chapter fade-ins, and footnote
pop-ins — all driven by `IntersectionObserver`, with the actual motion
handled by plain CSS transitions.

## How it works

### 1. Top progress bar

The bar's fill width is a straight percentage of scroll position,
recalculated on every `scroll` event:

```js
function updateProgress() {
  var scrollTop = window.scrollY || document.documentElement.scrollTop;
  var docHeight = document.documentElement.scrollHeight - window.innerHeight;
  var progress = docHeight > 0 ? (scrollTop / docHeight) * 100 : 0;
  fill.style.width = progress + "%";
}
```

A short CSS `transition: width 80ms linear` smooths out the steps
between scroll-event ticks so the bar doesn't visibly jump.

### 2. Chapter fade-ins

Each `<section data-chapter>` starts at `opacity: 0` with a slight
downward offset. An `IntersectionObserver` watches all chapters and
adds `.is-visible` the first time a chapter crosses 20% into view,
then stops watching it (one-shot reveal, not a scroll-jank loop):

```js
var chapterObserver = new IntersectionObserver(function (entries) {
  entries.forEach(function (entry) {
    if (entry.isIntersecting) {
      entry.target.classList.add("is-visible");
      chapterObserver.unobserve(entry.target);
    }
  });
}, { threshold: 0.2 });
```

The actual fade + rise is a CSS transition on `.chapter.is-visible`,
not anything animated from JS.

### 3. Footnote pop-ins

Footnotes (`<aside data-footnote>`) work the same way, but pop in with
a subtle `scale()` transition instead of a rise, and at a higher
visibility threshold (40%) so they don't trigger until they're mostly
on screen. Clicking a footnote's superscript number smooth-scrolls
that footnote into view.

## Reusing this pattern

To add a new coordinated scroll motion, pair one `IntersectionObserver`
per motion type with a one-line CSS transition:

```css
.your-element {
  opacity: 0;
  transition: opacity 500ms ease;
}
.your-element.is-visible {
  opacity: 1;
}
```

```js
var observer = new IntersectionObserver(function (entries) {
  entries.forEach(function (entry) {
    if (entry.isIntersecting) {
      entry.target.classList.add("is-visible");
      observer.unobserve(entry.target);
    }
  });
}, { threshold: 0.2 });

document.querySelectorAll(".your-element").forEach(function (el) {
  observer.observe(el);
});
```

## Accessibility

- `@media (prefers-reduced-motion: reduce)` disables every transition
  and shows chapters/footnotes at full opacity immediately — content
  is never hidden or delayed for users who've asked for less motion.
- Footnote references are focusable and keyboard-activatable
  (`:focus-visible` styling included).
- The progress bar is `aria-hidden`, since it's a supplementary visual
  cue, not essential reading content.

## Files

- `demo.html` — markup, sample long-form article content, and the
  `IntersectionObserver` orchestration script.
- `style.css` — progress bar, chapter fade, and footnote pop-in styling.
- `README.md` — this file.
