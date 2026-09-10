# Prompt — Prompt Like a Pro (Discovery, 10 pts)

> Quest: run the same task twice — one-line prompt vs. context + role + example — and compare.

## The situation

First day, so no real backlog task to use. Picked something every new hire
actually needs on day one: **a Slack introduction message for the team channel.**
Realistic, low-stakes, and the kind of thing people normally fire off in one line —
which makes it a good test case.

## Prompt A — one line

```
Write a Slack message introducing myself to the team on my first day.
```

Output: [`output-one-line.md`](output-one-line.md)

## Prompt B — role + context + task + example + constraints

```
You're a communications coach who helps new hires make a strong first impression.

Context: This is for a new hire posting in the company-wide #general Slack channel
on their first day. Nobody on the team has met them yet. The company works in
property management software. They want to sound approachable and a little
personable — not like a corporate press release — and to invite people to reach out
rather than reciting a resume.

Task: Draft a first-day Slack intro message.

Here's an example of the style I'm after: a short, warm post that leads with
something human (not a job title), mentions what they're looking forward to, and
ends with an open invitation to say hi — no bullet-pointed accomplishments.

Keep it to 100-150 words, 2-3 short paragraphs. Avoid corporate buzzwords
("synergy", "excited to leverage") and avoid reciting a full work history.
```

Output: [`output-structured.md`](output-structured.md)

## The difference

The one-line version is generic enough to have been written for anyone, at any
company, in any tone. It hits the literal ask ("introduce myself") and stops.

The structured version sounds like a message a specific person would post: a tone
constraint (approachable, not corporate), a shape constraint (short,
invitation-ended, no resume-reciting), and a style anchor (the example), so the
model isn't guessing what "good" means — it's told. Same model, same task, just a
better-specified request.

**The transferable bit:** the example field did more work than the role line. Saying
"you're a communications coach" changed little; showing the shape of a good answer
changed everything.

## Honest note

Both outputs still contain bracketed placeholders (`[something human — a hobby...]`)
because the actual personal details weren't supplied to the model. The structured
prompt produced a better *scaffold*, not a finished post — worth being clear about,
since a comparison quest can easily overstate how much the second prompt won by.
