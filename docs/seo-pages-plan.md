# Alpenglow SEO pages: implementation plan

Written 2026-10-27 from the "Alpenglow SEO Landing Pages" Google Doc
(https://docs.google.com/document/d/1I6e43KurCnQZaPu30rxCBDRrgNorXukN/edit)
and the "Website" email thread with Amber (Aug 21 to Oct 25, 2026).

## Where things stand

- Amber finished editing all six pages on Oct 1 and followed up on Oct 25 asking if you had looked. Your last reply was Sep 28, promising to publish pages 1 to 3 "today". Nothing has been published. Replying to her is the first action.
- The doc has 25 open suggestions, none accepted. Several are instructions in parentheses, not literal text: "(change to 'family member')", "(add ',PMH-C')", "(change to 'your extended family and inner-circle')", "(change to 'family members')". Implement the instruction, do not paste the parenthetical.
- Amber's two notes on page 1 of the doc:
  1. Mentaya widget. She has decided she can walk clients through checking out-of-network benefits herself and asks whether the calculator would scare people off before an intake call. She wants your opinion.
  2. PSI certification badge. Sent Sep 3, never added. She wants it in the footer on every page. Image: https://api.accredible.com/v1/frontend/credential_website_embed_image/badge/193037265
- Site drift the doc does not know about:
  - The /how/ retitle to "How I Can Help" from commit 96a2107 was undone when origin/main was merged in (77a7a37). /how/ is "My Approach" again, with Modalities, Clinical Supervision, "My Clients Often Struggle With", and "Step Into Change" sections. Page 7 of the doc assumes "How I Can Help" is live.
  - The homepage FAQ still says "I accept Blue Cross Blue Shield network plans and Medicaid/Green Mountain Care". Amber dropped insurance months ago. This contradicts every new page and must change in the same release.
  - The hub spec lists an "Online Therapy in Vermont" page. There is no draft for it in the doc.
  - The Google Analytics tag is pasted into four page files, not base.html. About has none. New pages would miss it unless it moves to the layout.
- Git: HEAD on ticket/alpenglow/address-update equals origin/main. Local main is stale. Branch the new work from HEAD.

## Build order (from the doc)

1 Perinatal hub, 2 Postpartum, 3 Pregnancy loss, 4 EMDR, 5 Supervision, 6 College students, 7 /how/ hub update and site wiring.

## Phase 0: prep (about 1 hour)

1. `git checkout -b ticket/alpenglow/seo-pages` from current HEAD.
2. Move the gtag snippet from the four page files into `src/_includes/layouts/base.html` head.
3. Create `src/assets/less/service.less`. Copy the `#int-hero` and `#content-page-1945` rules from about.less (the side-by-side image/text stitch Amber agreed to), add a small "related pages" link strip. Register it in the build the same way the other page sheets are (less-watch-compiler picks up the folder).
4. Footer: add the PSI badge image under the logo in `.left-section`, linking to the credential. Uncomment the Services `<ul>` and leave it ready for Phase 3 links.
5. Homepage FAQ: replace the BCBS/Medicaid answer with private-pay plus superbill wording. Draft it in the reply to Amber and let her confirm.

## Phase 1: pages 1 to 3 (about 3 hours)

One file each in `src/content/pages/`, modelled on about.html: front matter title, description, permalink from the spec; `#int-hero` with the H1; one or two `#content-page-1945` sections for the body; `{% include 'components/cta.html' %}`.

| # | File | Permalink | Hero/side image |
|---|------|-----------|-----------------|
| 1 | perinatal-mental-health-therapy-vermont.html | /perinatal-mental-health-therapy-vermont/ | amber-about.jpeg |
| 2 | postpartum-therapist-vermont.html | /postpartum-therapist-vermont/ | amber-clover.jpeg |
| 3 | pregnancy-loss-therapist-vermont.html | /pregnancy-loss-therapist-vermont/ | white-mt-1.jpeg |

Apply Amber's edits as you go:

- Page 1: "mother-in-law" becomes "family member"; "grandparents and in-laws" becomes "your extended family and inner-circle"; "Parents and in-laws" becomes "family members". Keep "in Vermont" in the first sentence (it is one of the five keep spots) and tell her why.
- Page 2: "I'm Amber, LCMHC, PMH-C."
- Page 3: "Empty Arms Vermont and Postpartum Support International run peer support groups state and nationwide".

Wire the [link: ...] markers: hub links down to 2, 3, and EMDR (EMDR goes live in Phase 2, so link it then or link now to the future URL). 2 and 3 link up to the hub. Every page ends with the consult CTA and the phone number.

Commit, push, open a PR, send Amber the deploy preview links. This is what you promised on Sep 28.

## Phase 2: pages 4 to 6 (about 3 hours)

| # | File | Permalink | Hero/side image |
|---|------|-----------|-----------------|
| 4 | emdr-therapy-richmond-vt.html | /emdr-therapy-richmond-vt/ | xc-tracks.jpeg |
| 5 | clinical-supervision-vermont.html | /clinical-supervision-vermont/ | amber-leaves.jpeg |
| 6 | therapy-for-college-students-vermont.html | /therapy-for-college-students-vermont/ | landing-about-1.jpeg |

Amber's edits:

- Page 4: "EMDR is used most often for trauma"; the sessions paragraph becomes "In our early sessions, we look at your history, your goals for therapy, and getting you settled in to the process."
- Page 5: drop ", and licensed clinicians who want ongoing consultation". The private-pay paragraph becomes "...a practice that doesn't tie you to insurance panels. While we remain clinically focused, we will also support you with learning to talk about fees...". Delete the whole "What I don't offer" section (her call; her remaining line about panels still signals it). Fees paragraph: "I offer individual supervision on a weekly basis. Many supervisees are required to receive near-weekly supervision to reach the required hours, so this is my typical structure. I meet with supervisees in-person and online. My rate is $100 per hour/weekly session."
- Page 6: replace "I don't diagnose ADHD or prescribe." with her sentence ("While an ADHD diagnosis will require further evaluation outside of our work together, we can work together to bring helpful structure back to your life and practice ongoing strategies so you remain in control."), then "If you need an evaluation, I can help you figure out where to get one through my referral network." "be in a room" becomes "meet with me in-person". Keep the school list unless she cuts it.

Also: the ski-EMDR paragraph on /approach/ gets a link to page 4.

## Phase 3: hub and wiring (about 2 hours)

1. `/how/`: restore the "How I Can Help" title, description, and H1 (copy from `git show 96a2107:src/content/pages/how.html`). Keep Modalities and Step Into Change. Replace "My Clients Often Struggle With" with the seven-item service block from page 7 of the doc, each heading linking to its page. Shrink the Clinical Supervision section to a short paragraph plus link, since page 5 now owns it. Rename the nav and footer label to "How I Can Help".
2. Footer Services column: the six live pages. Add "Online Therapy in Vermont" when that page exists.
3. Homepage: the four `#services-346` cards link to the specific pages instead of /how. "Personalized Care" section gets inline links to the perinatal and college pages.
4. About page: perinatal section links to page 1, college section links to page 6.
5. `npm run build`, then check the output: one H1 per page, title and meta match the spec, all seven URLs in public/sitemap.xml, no broken internal links (grep hrefs against the public/ tree).

## Phase 4: reply to Amber (draft below, you send)

Points to cover, in her order:

- Thanks, congrats received, apology for the gap.
- Pages 1 to 3 go up this week, 4 to 6 the week after, then the hub and footer.
- "In Vermont" stays in the first sentence. It is one of the five spots where the phrase has to appear word for word. Everything after that sentence is hers.
- Mentaya: agree with her. Skip the widget. Replace it with a plain FAQ answer on the homepage and a line on each page: private pay, superbill provided, and a short "how to check your out-of-network benefits" paragraph she can write. People who want to rule themselves out will still call first, which is the goal.
- PSI badge goes in the footer this round. Ask for the public credential page URL so the badge links somewhere.
- The homepage FAQ still says BCBS and Medicaid. Send her the replacement wording to confirm.
- Online Therapy in Vermont is the one page without a draft. Offer to write it next round.
- She does not need to accept the suggestions in the doc. You will build from them directly.

## Decisions for Sam

1. Supervision page: accept Amber's deletion of the "What I don't offer" insurance section? Recommend yes. It is her page and her remaining "doesn't tie you to insurance panels" line still filters.
2. /how/ nav label: "How I Can Help" (doc) or keep "My Approach"? Recommend "How I Can Help" so the hub name matches the H1.
3. Mentaya: recommend skip, per the reply above.
4. Online Therapy in Vermont: draft it next round as the one location-style page, per the house call against town pages.
5. Ship as three PRs (Phase 0+1, Phase 2, Phase 3) or one? Recommend three so Amber sees pages 1 to 3 this week.

## Time

About 9 to 10 hours of build across three PRs, plus Amber's review turns.
