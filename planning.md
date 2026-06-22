# TakeMeter Planning: r/Buddhism Post Classifier

## Community

r/Buddhism is an active Reddit community dedicated to Buddhist practice, philosophy, and discussion across all traditions. It is a strong fit for classification because the discourse varies enormously — from practical meditation questions to deep doctrinal debates to personal testimonials to resource sharing. A regular participant in this community would immediately recognize the difference between someone asking how to sit zazen versus someone sharing a personal breakthrough versus someone recommending Thich Nhat Hanh books. These distinctions are meaningful to the community, not just to an outside observer.

## Label Taxonomy

**practice** — the post asks about or describes a specific meditation technique, ritual, or observable method. The central question is how to do something.

Examples:
1. "I would consider that being bad at meditation is the point of meditation... Success is not found in the silence, but in the returning."
2. "The fundamental misunderstanding many beginners face is the belief that meditation is a state to be entered, rather than a process of observing the mind exactly as it is."

**philosophy** — the post engages with Buddhist doctrine, texts, concepts, or tradition. The focus is on understanding what something means, not how to practice it.

Examples:
1. "Buddhism, at its core, is in fact a religion. Not a philosophy... To alter the teachings of the Buddha to fit a modern understanding, rejecting the history and wisdom of a 2400 year old tradition is tremendously offensive."
2. "700 million people in this world live in extreme poverty... That will definitely be us in a future life just by mathematical certainty if we don't enter the teachings of Buddha."

**personal** — the post shares the author's own experience, struggle, breakthrough, or life situation. The author is the subject, not a teaching or technique.

Examples:
1. "I've been reading Buddhist texts about Buddha's life... I cannot help myself but find all of it as some sort of religious fantasy."
2. "I am born in a Buddhist family... Within 4 days I went through severe anxiety, panic attack and i literally disconnected from the world. And I am really happy now."

**resource** — the post primarily recommends or requests books, teachers, podcasts, apps, or links. The value is the pointer to external content, not the discussion itself.

Examples:
1. "Thanissaro Bhikkhu has wonderful talks that he gives daily... Ajahn Achalo: I really like his talks... Venerable Robina Courtain is from a Tibetan perspective but has many talks concerning the basics."
2. "https://www.audiodharma.org/ — There is a Path Fundamentals tab where the focus of those talks is more on the core teachings of Buddhism."

## Hard Edge Cases

**practice vs personal:** A post describing how the author meditates can be either — if the central purpose is sharing a technique others can use, it is practice. If the central purpose is sharing the author's inner experience with technique as secondary, it is personal.

Example: "My weird meditation practice" (rock concert meditation) → practice, because the post describes a method and invites others to try or discuss it.

**resource vs philosophy:** A post recommending a book but also explaining its doctrinal significance could be either. Decision rule: if the post's primary value is the pointer to external content, label it resource. If the recommendation is incidental and the post is primarily explaining ideas, label it philosophy.

## Data Collection Plan

Source: r/Buddhism text posts and comments, filtered to text-only.
Target: 200 examples minimum, aiming for 50 per label (balanced distribution).
Collection method: Manual copy-paste into Google Sheets with columns: text, label.
If a label is underrepresented after 150 examples: search r/Buddhism specifically for that label type using targeted keywords (e.g., "recommend" for resource, "my experience" for personal).
Exclusions: image-only posts, posts under 20 words, mod announcements, pinned posts.

## Evaluation Metrics

**Overall accuracy** — fraction of test examples classified correctly. Necessary but not sufficient.

**Per-class accuracy** — accuracy broken down by label. Essential because a classifier that always predicts "personal" could achieve high overall accuracy if personal posts dominate the dataset, while completely failing on philosophy and resource.

**Confusion matrix** — shows which labels are confused with which others. Directional errors (e.g., practice → personal more than personal → practice) reveal which boundary is underspecified.

**F1 score per class** — balances precision and recall, especially important if label distribution is uneven.

## Definition of Success

A classifier is genuinely useful if it achieves at least 70% overall accuracy and at least 60% per-class accuracy on all four labels. A model that hits 85% overall but scores below 50% on any single class is not acceptable — it means one label is essentially unlearned. For deployment in a real community tool, I would require at least 75% overall accuracy with no class below 65%.

## AI Tool Plan

**Label stress-testing:** Give Claude all four label definitions and ask it to generate 10 posts that sit at the boundary between practice and personal (the hardest pair). If I cannot classify the generated posts cleanly, tighten the definitions before annotating.

**Annotation assistance:** Claude generated all 200 synthetic training examples 
(50 per label) due to accessibility constraints. All examples were reviewed for 
label accuracy and authenticity before use. This will be disclosed in the AI 
Usage section of the README.

**Failure analysis:** After evaluation, I will give Claude the list of wrong predictions and ask it to identify patterns — for example, whether misclassifications cluster around short posts, posts mixing two topics, or a specific label pair. I will verify any pattern it identifies by reading the examples myself before including it in the evaluation report.