**WhatsApp QR Code Clone — Week 2 Improvements**  
*Product Requirements Document: New Feature*  
   
**Feature name:**  Goal-Gradient Progress Bar \+ Premium Feature Unlock  
**Owner:**  Chris Wozniak & Nathan Hutton  
**Date:**  June 13, 2026  
**Based on:**  WhatsApp QR Code Clone Week 1 PRD | AI-Native L2 Program  
 

# **1\. PROBLEM**

Free users of the WhatsApp QR Code Clone hit a 3-generation freemium gate with no visual warning it is coming, experience the wall as a surprise rather than a natural next step, and have no real premium features to unlock when prompted — resulting in a weak freemium conversion moment and an incomplete premium experience.  
 

## **1a. Background & Dependencies**

This PRD builds directly on the Week 1 WhatsApp QR Code Clone PRD (Chris Wozniak & Nathan Hutton, June 6, 2026). All Week 1 P0 features are shipped and working:  
•   	URL/phone number input → instant QR code generation  
•   	PNG download  
•   	Freemium counter (text-based, visible at all times)  
•   	Upgrade prompt triggered at 3 generations  
•   	WhatsApp green visual design (\#25D366)  
   
Week 1 deferred features now being built in Week 2:  
•   	Real premium unlock flow (upgrade button was mocked)  
•   	Custom QR code color picker  
•   	Logo/icon upload to center of QR code  
•   	SVG and PDF download formats  
•   	Live QR preview as user types  
•   	Goal-Gradient progress bar for freemium counter (new — added from UX research)  
   
Dependencies:  
•   	qrcode.js library (already integrated in Week 1\)  
•   	LocalStorage for tracking free usage count across sessions  
•   	File input API for logo upload feature  
 

## **1b. Target Use Cases**

•   	As a small business owner, I want to see how many free codes I have left visually so that I know the upgrade is coming before I hit the wall.  
•   	As a freelancer, I want to customize my QR code colors and add my logo so that the code matches my brand on business cards.  
•   	As a creator, I want to upgrade and generate unlimited codes so that I don't have to restart my session every time I hit the limit.  
•   	As a premium user, I want to download my QR code as SVG or PDF so that it prints at any size without losing quality.  
 

## **1c. Current User Journey**

Current flow (Week 1 build):  
•   	User opens the app and sees a text counter: '0 of 3 free codes used'  
•   	User enters a phone number and clicks Generate — QR code appears  
•   	User generates a second code — counter updates to '2 of 3'  
•   	User generates a third code — counter updates to '3 of 3'  
•   	→ Problem: No visual urgency — the text counter doesn't create any sense of approaching a limit  
•   	User tries to generate a 4th code — upgrade prompt appears  
•   	→ Problem: The wall feels like a punishment, not a natural next step  
•   	User sees an upgrade button — clicks it  
•   	→ Problem: Nothing happens. The upgrade flow is fully mocked. No premium features unlock.  
•   	→ Problem: Premium tier has no real value yet — no custom colors, no logo, no SVG/PDF download  
 

# **2\. PROPOSED SOLUTION**

Replace the text-only freemium counter with a Goal-Gradient visual progress bar that fills as users approach their free limit, making the upgrade feel like a natural next step. Simultaneously unlock the full premium experience — real color customization, logo upload, and SVG/PDF download — so the upgrade prompt leads somewhere worth going.  
   
**How it works:**  As users generate QR codes, a green progress bar fills from left to right — 0%, 33%, 66%, 100%. At 100% the bar turns amber and the upgrade prompt appears with a clear list of premium features. When a user clicks Upgrade, the premium tier unlocks in-session — they can immediately customize colors, upload a logo, and download in SVG or PDF format.  
 

## **2a. Value Proposition**

Small business owners and creators who use our WhatsApp QR Code Clone use the new Goal-Gradient progress bar and premium unlock flow to approach and cross the freemium gate with confidence rather than surprise. Unlike the Week 1 text-only counter and mocked upgrade button, the progress bar creates visual urgency that motivates upgrade, and the premium features deliver real value the moment the user pays — helping them walk away with a branded, print-ready QR code that represents their business.  
 

## **2b. Goals & Out-of-Scope**

### **Goals**

•   	Make the freemium gate feel motivating not punishing — users should see it coming and want to cross it  
•   	Deliver a real premium experience — at least 3 working premium features available immediately after upgrade  
•   	Increase demo impact — judges should see a complete freemium product loop, not a mocked one  
•   	Apply Laws of UX learned in Week 2 — Goal-Gradient Effect directly implemented in the product  
 

### **Out-of-Scope**

•   	Real payment processing — upgrade still simulated in-session, no Stripe integration this week  
•   	Scan analytics and tracking — deferred to v3  
•   	User accounts or persistent login — out of scope for Cycle 1  
•   	Mobile app — web only  
 

## **2c. Measurable Outcomes**

| Metric | How it's measured | Baseline | Target |
| :---- | :---- | :---- | :---- |
| Freemium upgrade rate | % of users who click upgrade after hitting gate | 0% (mocked in Week 1\) | \>15% of sessions |
| Progress bar engagement | Users who notice counter before hitting limit | Not tracked in Week 1 | 100% awareness before gate |
| QR scan success rate | Successful WhatsApp opens on real device | 3 scans in Week 1 demo | 5+ scans in Week 2 demo |
| Premium feature usage | Custom color / logo used per session | 0 (not built in Week 1\) | Used in every demo |

 

# **3\. REQUIREMENTS**

## **User Journey 1: Free user approaching the freemium gate**

Context: A small business owner is generating QR codes and needs to feel the upgrade coming naturally, not hit a wall without warning.  
 

### **Sub-journey: Goal-Gradient progress bar**

•   	**\[P0\]**  User can see a visual progress bar that starts empty and fills as they use free generations.  
•   	**\[P0\]**  Progress bar shows 0% on load, 33% after 1st generation, 66% after 2nd, 100% after 3rd.  
•   	**\[P0\]**  Progress bar color is WhatsApp green (\#25D366) from 0–66% and turns amber (\#FFA500) at 100%.  
•   	**\[P0\]**  Text label below bar shows remaining count (e.g. '1 free code remaining').  
•   	**\[P1\]**  Progress bar animates smoothly on each fill increment.  
•   	**\[P2\]**  Micro-copy changes as user approaches limit — e.g. 'Last free code\!' at 66%.  
 

### **Sub-journey: Upgrade prompt**

•   	**\[P0\]**  At 100% bar fill, user is shown upgrade prompt with clear list of what premium unlocks.  
•   	**\[P0\]**  Upgrade prompt includes: unlimited codes, custom colors, logo upload, SVG/PDF download.  
•   	**\[P0\]**  User can click Upgrade and immediately access premium features in-session (simulated unlock).  
•   	**\[P1\]**  Upgrade prompt design matches WhatsApp visual style — green CTA button, clean card layout.  
•   	**\[P2\]**  User can enter email to simulate account creation during upgrade.  
 

## **User Journey 2: Premium user customizing and downloading a QR code**

Context: A freelancer who has upgraded wants a branded QR code that matches their business identity and prints at high quality.  
 

### **Sub-journey: Custom color picker**

•   	**\[P0\]**  Premium user can select a custom foreground color for the QR code using a color picker.  
•   	**\[P0\]**  QR code updates in real time as user selects a new color.  
•   	**\[P1\]**  Color picker includes WhatsApp green as a default/suggested option.  
•   	**\[P2\]**  User can enter a hex color code directly.  
 

### **Sub-journey: Logo upload**

•   	**\[P0\]**  Premium user can upload a PNG or JPG logo that appears centered on the QR code.  
•   	**\[P0\]**  Logo is sized automatically to fit within the QR code's quiet zone without breaking scannability.  
•   	**\[P1\]**  User can remove the logo and revert to a plain QR code.  
•   	**\[P2\]**  User sees a warning if the logo size may affect scan reliability.  
 

### **Sub-journey: SVG and PDF download**

•   	**\[P0\]**  Premium user can download the QR code as an SVG file in addition to PNG.  
•   	**\[P1\]**  Premium user can download the QR code as a PDF file.  
•   	**\[P1\]**  Download format selector is clearly labeled with use-case hints (e.g. 'SVG — best for print').  
•   	**\[P2\]**  Downloaded files include the user's custom color and logo if applied.  
 

### **Sub-journey: Live preview**

•   	**\[P0\]**  All users (free and premium) can see a live QR code preview that updates as they type.  
•   	**\[P1\]**  Preview updates with a short debounce (300ms) to avoid re-rendering on every keystroke.  
•   	**\[P2\]**  Preview shows a placeholder state before a valid input is entered.  
 

# **4\. APPENDIX**

## **Design Decisions**

•   	Decision: Use Goal-Gradient Effect (Laws of UX) for the progress bar rather than a simple numeric counter. Rationale: Research shows users accelerate toward a goal as they see it approaching — a visual bar creates urgency and motivation that text alone does not.  
•   	Decision: Simulate premium unlock in-session without real payments. Rationale: Building Stripe integration is out of scope for a one-week cycle — demonstrating the full premium experience is more valuable for the demo than wiring up real payments.  
•   	Decision: Logo upload centers the image within the QR code's quiet zone. Alternative considered: overlay logo on top of the entire code. Rejected because it breaks scannability.  
 

## **Open Questions**

•   	Does the premium unlock persist across browser sessions, or reset on page reload? Owner: Chris — decide before build kickoff.  
•   	Should the color picker be a native HTML input or a custom component? Owner: Nathan — evaluate complexity before starting.  
•   	At what logo size does the QR code become unscannable? Owner: Chris — test during build and set a safe maximum.  
 

## **Other Links**

•   	Related PRD: WhatsApp QR Code Clone Week 1 PRD — Chris Wozniak & Nathan Hutton, June 6, 2026  
•   	UX Reference: Laws of UX — Goal-Gradient Effect (lawsofux.com)  
•   	Tech Stack: qrcode.js, HTML/CSS/JS, VS Code, deployable to Vercel  
•   	Color reference: WhatsApp green \#25D366, Amber gate color \#FFA500  
   
   
*PRD Owner: Chris Wozniak & Nathan Hutton  |  Cycle 1 Week 2  |  AI-Native L2 Program*  
