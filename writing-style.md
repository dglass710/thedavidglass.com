# David Glass — Writing Style Guide

## Deduced from analysis of 13 blog posts (Oct 2023 – Jun 2024)

---

## Voice & Perspective

- First person singular ("I designed", "I developed", "I exploited")
- Professional but approachable — not stiff academic, not casual blog
- Speaks as a practitioner, not a student — frame everything as professional work, not coursework
- Confident without being boastful — let the work speak, don't over-explain why it matters

## What to Keep

- **Specificity over generality**: The best parts of the writing name exact tools (Metasploit, Burp Suite, John the Ripper, Aircrack-ng), exact techniques (DCSync, SQL injection, LFI), and exact outcomes (cracked passwords, captured flags, exploited CVEs). This is the strongest signal in the writing — lean into it harder.
- **Structured project write-ups**: The Project Overview → Details → Outcome format works well for case studies. Employers can skim.
- **Technical confidence**: Comfortable discussing network topologies, Docker, Ansible, Azure NSGs, SIEM, Splunk, bash scripting. Doesn't shy away from depth.
- **Practical framing**: Always ties back to real-world application, not just theory.

## What to Fix

### 1. Kill the AI filler phrases
The biggest tell. These add zero information and make every post sound the same:
- "This project demonstrates my ability to..."
- "This experience is a testament to my capabilities in..."
- "showcasing my skills in developing and maintaining..."
- "has been incredibly enriching"
- "was an exhilarating experience"
- "has transformed my understanding"
- "equipped me with both technical and strategic skills"

**Rule: Never end a section by telling the reader what skill it demonstrated. If you did the work, they can see the skill.**

### 2. Cut the adjective inflation
Almost every experience is "enlightening", "exhilarating", "immensely rewarding", "incredibly enriching", "intensive and fulfilling". When everything is superlative, nothing is.

**Rule: Use plain language. "I learned X" beats "I embarked on an enlightening journey into X". One strong adjective per post, max.**

### 3. Stop restating the same point
Many paragraphs say the same thing twice in different words. Example:
> "By using a jump box with restricted access and managing applications in isolated Docker containers, the setup protected against various security threats."
> Then immediately: "network security groups were meticulously configured to ensure that only authorized traffic could access critical resources, safeguarding the entire infrastructure."

Both say "security was enforced at multiple layers." Say it once, concretely.

**Rule: One idea, one sentence. If a paragraph repeats itself, cut the weaker version.**

### 4. Make conclusions earn their space
Nearly every post ends with a variation of "This demonstrated my ability to X, preparing me for Y." These are interchangeable — you could swap any post's conclusion onto any other post.

**Rule: Conclusions should say something specific to THIS project. What was the hardest part? What would you do differently? What's the one takeaway someone should remember? If the conclusion could apply to any project, delete it.**

### 5. The bootcamp posts are too thin
Posts like boot-camp-0 through boot-camp-3 are single paragraphs that read like social media captions, not blog entries. They list topics covered but don't show any depth or personality.

**Rule: If a post can't stand on its own with at least one concrete example, specific takeaway, or honest reflection — expand it or merge it with adjacent entries.**

### 6. Passive voice creep
"Was configured to", "were specifically configured", "was particularly instrumental in enhancing". This distances the author from the work.

**Rule: Default to active voice. "I configured the jump box to..." not "The jump box was configured to..."**

## Tone Targets

- **Direct**: Get to the point. The reader is a hiring manager with 30 seconds.
- **Concrete**: Tools, techniques, outcomes. Not feelings about the experience.
- **Honest**: It's fine to say something was hard or that you learned from a mistake. That's more credible than "every experience was incredibly enriching."
- **Measured**: Professional confidence, not resume-speak. Write like you're explaining the project to a sharp colleague, not pitching yourself in a cover letter.

## Structure Preferences

- **Case studies**: Overview → What I Did (with specifics) → Challenges → Results. Keep bold-label bullet lists for tools/techniques.
- **Development series**: Can be more narrative/reflective, but still needs concrete anchors — specific concepts learned, specific exercises done, specific "aha" moments.
- **Paragraphs**: 2-4 sentences. No walls of text.
- **Headers**: Clean, descriptive. No bold-wrapping headers (the markdown `##` is enough).
