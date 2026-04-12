# Design System Strategy: The Cinematic Negative
 
## 1. Overview & Creative North Star
**Creative North Star: "The Digital Gallery"**
 
This design system is predicated on the concept of the website as a curated physical space. We are moving away from the "web template" mindset and toward a "high-end editorial" experience. Every pixel must feel as intentional as a frame of 35mm film. 
 
To achieve this, we leverage **intentional asymmetry** and **expansive void**. We do not fill space; we allow space to breathe, drawing the eye toward the photography. The layout should mimic the experience of a darkroom or a high-end gallery—high contrast, deep blacks, and razor-sharp typography. By using sharp 0px corners and a monochromatic palette, we create a sense of permanence and architectural rigor.
 
---
 
## 2. Colors: Tonal Architecture
The palette is not merely "black and white"—it is a study in light and shadow. We use the Material Design tokens to create a sophisticated, layered environment.
 
### The "No-Line" Rule
Traditional 1px borders are strictly prohibited for sectioning. They create a "boxed-in" feeling that ruins the cinematic flow. Instead, boundaries are defined by:
- **Tonal Shifts:** Transitioning from `surface` (#131313) to `surface_container_low` (#11b1b1b) to define a new content area.
- **Negative Space:** Using large blocks of `background` to separate editorial narratives.
 
### Surface Hierarchy & Nesting
Treat the UI as physical layers. Content hierarchy is achieved by "stacking" tones:
*   **Base Layer:** `surface` (#131313)
*   **Secondary Content:** `surface_container` (#1f1f1f)
*   **Elevated Elements:** `surface_container_high` (#2a2a2a)
 
### Glass & Gradient Rule
To prevent the design from feeling "flat" or "cheap," floating navigation or overlay cards must use **Glassmorphism**. Utilize `surface` at 70% opacity with a `20px backdrop-blur`. 
*   **Signature Polish:** For primary call-to-actions, apply a subtle linear gradient from `primary` (#ffffff) to `primary_container` (#d4d4d4) at a 45-degree angle to provide a metallic, premium sheen.
 
---
 
## 3. Typography: Editorial Authority
The typographic pairing is a conversation between heritage and modernity.
 
*   **Display & Headlines (Newsreader):** This serif font provides an "Old World" luxury feel. Use `display-lg` (3.5rem) with increased letter-spacing (-0.02em) to create an authoritative, cinematic title.
*   **Body & Labels (Manrope):** This clean sans-serif ensures maximum legibility. It acts as the "caption" to the photography.
*   **Visual Hierarchy:** Titles should often be center-aligned or offset to the far left to break the standard grid, while body text should remain in narrow, readable columns (max-width: 60ch).
 
---
 
## 4. Elevation & Depth: Tonal Layering
In a "zero-radius" system, shadows and borders must be handled with extreme delicacy to avoid looking dated.
 
*   **The Layering Principle:** Instead of a shadow, place a `surface_container_highest` element on top of a `surface_dim` background. The subtle 2-3% difference in hex value creates "Ambient Depth."
*   **Ambient Shadows:** If a floating element (like a modal) is required, use a shadow with a blur of `60px` and an opacity of `8%` using the `on_surface` color as the tint. This mimics light bouncing off a matte surface.
*   **The Ghost Border:** If a form field or button needs a perimeter, use the `outline_variant` token at **15% opacity**. It should be felt, not seen.
*   **Glassmorphism:** Apply to the Header/Navigation bar. This allows photography to "ghost" through the UI as the user scrolls, maintaining the cinematic immersion.
 
---
 
## 5. Components
 
### Buttons (The Minimalist Trigger)
*   **Primary:** Solid `primary` (#ffffff) with `on_primary` (#1a1c1c) text. 0px corner radius. No border.
*   **Secondary:** Ghost style. No background. 1px "Ghost Border" (15% opacity `outline`). Text in `primary`.
*   **Interaction:** On hover, the Secondary button fills with `primary` and text flips to `on_primary`. The transition must be a slow, 400ms ease.
 
### Input Fields (Editorial Forms)
*   **Style:** Underline only. Use `outline` token for the bottom border (1px).
*   **State:** When focused, the border transitions to `primary` (#ffffff) and a subtle `surface_container_high` background fades in.
*   **Labels:** Use `label-md` in all-caps with 0.1rem letter spacing, positioned above the input.
 
### Cards & Image Containers
*   **Rule:** **No Divider Lines.** 
*   **Spacing:** Use 80px - 120px of vertical white space to separate gallery items.
*   **Captions:** Use `body-sm` or `label-md` aligned to the bottom right of the image to mimic a museum plaque.
 
### Additional Component: The "Cinematic Scrubber"
For photography portfolios, replace standard progress bars with a 1px `primary` line that spans the full width of the viewport, indicating scroll depth or gallery progress.
 
---
 
## 6. Do's and Don'ts
 
### Do:
*   **Embrace the Void:** Let images sit in large fields of `surface_container_lowest`.
*   **Use Intentional Asymmetry:** Align text to the left and images to the right, or vice versa, to create a rhythmic, non-linear flow.
*   **High-Contrast Media:** Ensure photography is edited to complement the deep black (`#131313`) background.
 
### Don't:
*   **No Rounded Corners:** Ever. The `0px` rule is absolute to maintain the architectural, high-end feel.
*   **No Standard Grids:** Avoid the "3-column card row." Try a "1-2-1" staggered layout to feel more editorial.
*   **No Heavy Borders:** Never use a 100% opaque border. It breaks the "Digital Gallery" immersion and makes the site look like a legacy bootstrap site.
*   **No Pure Grey Shadows:** Always tint shadows with the `on_surface` color to maintain tonal warmth.