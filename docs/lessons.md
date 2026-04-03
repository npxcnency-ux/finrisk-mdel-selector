Don’t State the Obvious
Claude Code knows a lot about your codebase, and Claude knows a lot about coding, including many default opinions. If you’re publishing a skill that is primarily about knowledge, try to focus on information that pushes Claude out of its normal way of thinking.
The frontend design skill is a great example — it was built by one of the engineers at Anthropic by iterating with customers on improving Claude’s design taste, avoiding classic patterns like the Inter font and purple gradients.
Build a Gotchas Section
The highest-signal content in any skill is the Gotchas section. These sections should be built up from common failure points that Claude runs into when using your skill. Ideally, you will update your skill over time to capture these gotchas.
Use the File System & Progressive Disclosure
Like we said earlier, a skill is a folder, not just a markdown file. You should think of the entire file system as a form of context engineering and progressive disclosure. Tell Claude what files are in your skill, and it will read them at appropriate times.
The simplest form of progressive disclosure is to point to other markdown files for Claude to use. For example, you may split detailed function signatures and usage examples into references/api.md.
Another example: if your end output is a markdown file, you might include a template file for it in assets/ to copy and use.
You can have folders of references, scripts, examples, etc., which help Claude work more effectively.
Avoid Railroading Claude
Claude will generally try to stick to your instructions, and because Skills are so reusable you’ll want to be careful of being too specific in your instructions. Give Claude the information it needs, but give it the flexibility to adapt to the situation. For example:
Think through the Setup
Some skills may need to be set up with context from the user. For example, if you are making a skill that posts your standup to Slack, you may want Claude to ask which Slack channel to post it in.
A good pattern to do this is to store this setup information in a config.json file in the skill directory like the above example. If the config is not set up, the agent can then ask the user for information.
If you want the agent to present structured, multiple choice questions you can instruct Claude to use the AskUserQuestion tool.
The Description Field Is For the Model
When Claude Code starts a session, it builds a listing of every available skill with its description. This listing is what Claude scans to decide "is there a skill for this request?" Which means the description field is not a summary — it's a description of when to trigger this PR.
Memory & Storing Data
Some skills can include a form of memory by storing data within them. You could store data in anything as simple as an append only text log file or JSON files, or as complicated as a SQLite database.
For example, a standup-post skill might keep a standups.log with every post it's written, which means the next time you run it, Claude reads its own history and can tell what's changed since yesterday.
Data stored in the skill directory may be deleted when you upgrade the skill, so you should store this in a stable folder, as of today we provide `${CLAUDE_PLUGIN_DATA}` as a stable folder per plugin to store data in.
Store Scripts & Generate Code
One of the most powerful tools you can give Claude is code. Giving Claude scripts and libraries lets Claude spend its turns on composition, deciding what to do next rather than reconstructing boilerplate.
For example, in your data science skill you might have a library of functions to fetch data from your event source.  In order for Claude to do complex analysis, you could give it a set of helper functions like so:
Claude can then generate scripts on the fly to compose this functionality to do more advanced analysis for prompts like “What happened on Tuesday?”
On Demand Hooks
Skills can include hooks that are only activated when the skill is called, and last for the duration of the session. Use this for more opinionated hooks that you don’t want to run all the time, but are extremely useful sometimes.
For example:
/careful — blocks rm -rf, DROP TABLE, force-push, kubectl delete via PreToolUse matcher on Bash. You only want this when you know you're touching prod — having it always on would drive you insane
/freeze — blocks any Edit/Write that's not in a specific directory. Useful
when debugging: "I want to add logs but I keep accidentally 'fixing' unrelated