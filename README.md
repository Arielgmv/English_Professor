# The Professor — Learn what you already have saved

A private tutor that lives inside your system. It reads your notes, documents, and half-finished courses, and teaches you from there: it asks you before explaining, doesn't give you the answer but helps you find it, tells you why you got it wrong, and keeps track of what you know and what you still don't. And when needed, it prepares a dynamic explanation or a set of exercises on screen.

It doesn't study for you. It takes away your option to lie to yourself.

## What's inside
- `professor.md` — The skill in plain text. This is what you install.
- `engine.md` — Instructions for your AI to build "The Classroom": the screen where it prepares dynamic explanations and exercises. Not for you to read or touch.
- `profile.md` — Who you are, what you want to learn, why, and how you learn best.
- `learnings.md` — The map of what you are learning and the state of each part.
- `fails.md` — Your pile of failures: what you got wrong and when it returns.
- `reviews.md` — The review calendar for what you already get right.

## Installation in 4 Steps
1. **Fill in the brackets.** Open `profile.md` and fill in your details. It takes 5 minutes. If you skip it, The Professor will ask you three questions in the first conversation and fill it out for you.
2. **Choose your path:**
   - **If you use Claude (Ideal):** This is already a Claude Skill. In claude.ai: Settings → Skills → upload this ZIP as is. In Claude Code: unzip the `professor/` folder inside `.claude/skills/` of your project. The data files (`profile.md`, `learnings.md`, `fails.md`, `reviews.md`) go to your working folder (your `English_Professor` folder). `engine.md` stays inside the skill folder.
   - **If you use another AI (Gemini, ChatGPT, etc.):** Create a new project, name it "The Professor", paste the content of `professor.md` into the system instructions, and upload `engine.md` and the four data files as project knowledge. Also upload the material you want to learn from.
3. **Clear the examples.** `learnings.md`, `fails.md`, and `reviews.md` come with a fake Excel course so you can see the format. Delete it, or your AI will think you are studying pivot tables. *(Already done in the files below).*
4. **Start with a phrase.** Open the conversation and say: "I want to learn this. Look at what I have saved and ask me." It will read your files, tell you what it found, and begin.

## How to use it from then on
Talk to it like a private tutor:
- "What's on the agenda today?"
- "I don't understand this, explain it to me."
- "Prepare a test on Unit 4."
- "I only have 10 minutes."
- "Show me how I'm progressing."

It decides where to continue by looking at your files, not by guessing. What you fail returns tomorrow. What you get right returns increasingly later. And everything is logged.

## The Screen (The Classroom)
When an explanation is better seen than read, or when it's time for a set of exercises, it prepares it on screen: flip cards, step-by-step diagrams, quizzes with reasoned correction, fill-in-the-blanks, ordering, matching, and your progress panel.

- **In Claude Code:** The first time, it builds a small, private web app on your computer, "The Classroom". It does everything: installs what's missing, starts it, opens your browser, and shuts it down when done. You don't touch the terminal. No accounts, no external services, and your notes never leave your computer. What you answer is saved in your folder, and everything it prepares stays there for another day.
- **In claude.ai:** It prepares it as an interactive artifact within the conversation.
- **In another AI (like Gemini):** It generates a single, self-contained interactive HTML file that opens in your browser. When finished, you paste your results back into the chat, and it updates your files.

## ⚠️ Warning
It is an AI, and AIs make mistakes, sometimes with great confidence. That is why it teaches from *your* files and tells you where each thing comes from, and why it will warn you when it says something from memory. When something is at stake (an exam, a work decision), contrast it with the source: the manual, the official syllabus, the book. It will tell you where to look.