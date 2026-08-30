# Dom-Inspector

A single-file, dependency-free vanilla JS tool that lets you click any element on a page and copy a structured, DevTools-style snapshot of it (selector, DOM path, attributes, computed style, outerHTML) straight to your clipboard.

## Why this exists

When you ask an LLM to fix or restyle a specific element ("make this button bigger", "why is this div overflowing"), the model usually only has your vague description to go on — it doesn't know the element's real selector, its current computed styles, or its exact markup. That ambiguity leads to guessed selectors and fixes that target the wrong node.

`inspector.js` closes that gap: you click the element you mean, and a clean, structured text block describing exactly that element is copied to your clipboard. Paste it into the chat alongside your request ("fix **this** element: ...") and the LLM has precise, unambiguous context — real selector, real DOM position, real computed styles — instead of having to infer them from a screenshot or a natural-language description.

## How to use it

1. Add the script to your page, after your other scripts:

   ```html
   <script src="/inspector.js"></script>
   ```

   No build step, no dependencies — it's a single self-contained IIFE.

2. Toggle inspect mode on/off either by:
   - Clicking the small circular button that appears in the bottom-right corner of the page, or
   - Pressing **Alt+Shift+C** anywhere on the page.

3. While inspect mode is active, hover over the page — the element under your cursor is highlighted with an outline and a floating label showing its tag, id, classes, and size.

4. Click the element you want. The click is intercepted (it won't trigger the element's normal behavior), and a structured snapshot of that element is copied to your clipboard automatically. A small toast confirms the copy, and the snapshot is also added to a floating panel of recent captures (last 8), each of which can be re-copied with one click.

5. Press **Escape** at any time to exit inspect mode.

## Example

Clicking a button like this:

```html
<button id="submit-btn" class="btn btn-primary large">Submit</button>
```

copies something like this to your clipboard:

```
🔍 Inspected Element
Tag: <button id="submit-btn">
Selector: #submit-btn
DOM Path: form.checkout-form > div.form-actions > button#submit-btn.btn.btn-primary
Classes: btn btn-primary large
Attributes: id="submit-btn", class="btn btn-primary large", type="submit"
Text: "Submit"
Rect: 120x44 at (860, 512)
Computed Style:
  display: inline-flex
  position: static
  width: 120px
  height: 44px
  color: rgb(255, 255, 255)
  backgroundColor: rgb(37, 99, 235)
  border: 1px solid rgb(37, 99, 235)
  borderRadius: 8px
  fontSize: 16px
  fontWeight: 600
  cursor: pointer
Outer HTML:
<button id="submit-btn" class="btn btn-primary large" type="submit">Submit</button>
```

That block is enough for an LLM to know exactly which element you mean, what it currently looks like, and where it sits in the page — no back-and-forth needed.
