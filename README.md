import json
import os
import random

FILENAME = "prompts.json"

# -------------------- DATA --------------------
def load_prompts():
    if os.path.exists(FILENAME):
        with open(FILENAME, "r", encoding="utf-8") as f:
            return json.load(f)
    return []

def save_prompts(prompts):
    with open(FILENAME, "w", encoding="utf-8") as f:
        json.dump(prompts, f, indent=4, ensure_ascii=False)

# -------------------- FEATURES --------------------
def create_prompt(prompts):
    print("\n🧠 Create a New Prompt")
    topic = input("🎯 Topic (e.g. design, marketing, coding): ").strip()
    style = input("🎨 Style (e.g. professional, creative, friendly): ").strip()
    goal = input("🏁 Goal (What should AI do?): ").strip()
    tone = input("🎭 Tone (formal, funny, motivational, etc.): ").strip()
    keywords = input("🔑 Important keywords (comma separated): ").strip().split(',')

    template = f"""\
You are an expert in {topic}.
Your task: {goal}.
Use a {style} and {tone} tone.
Focus on these keywords: {', '.join([k.strip() for k in keywords])}.
Be concise, clear, and helpful.
"""

    print("\n✨ Generated Prompt:\n")
    print(template)

    prompts.append({
        "topic": topic,
        "style": style,
        "goal": goal,
        "tone": tone,
        "keywords": keywords,
        "prompt": template
    })
    save_prompts(prompts)
    print("\n✅ Prompt saved successfully!")

def view_prompts(prompts):
    if not prompts:
        print("\n📭 No prompts saved yet.")
        return
    print("\n=== 💾 Your Saved Prompts ===")
    for i, p in enumerate(prompts, 1):
        print(f"{i}. {p['topic']} → {p['goal']} ({p['style']}, {p['tone']})")

def suggest_prompt():
    templates = [
        "Write a creative story about {topic} in a {tone} tone.",
        "Generate 5 marketing slogans related to {topic}.",
        "Explain {topic} in a simple, beginner-friendly way.",
        "Create a persuasive product description for {topic}.",
        "Write a motivational post about {topic} using a {style} writing style."
    ]
    topic = input("💡 Enter a topic: ").strip()
    tone = random.choice(["friendly", "professional", "funny", "motivational"])
    style = random.choice(["creative", "informative", "emotional"])
    prompt = random.choice(templates).format(topic=topic, tone=tone, style=style)
    print("\n⚡ Suggested Prompt:\n")
    print(prompt)

def main():
    prompts = load_prompts()
    while True:
        print("\n=== 🤖 PROMPT WRITING ASSISTANT ===")
        print("1️⃣ Create new prompt")
        print("2️⃣ View saved prompts")
        print("3️⃣ Get random prompt suggestion")
        print("4️⃣ Exit")

        choice = input("👉 Choose an option: ").strip()
        if choice == "1":
            create_prompt(prompts)
        elif choice == "2":
            view_prompts(prompts)
        elif choice == "3":
            suggest_prompt()
        elif choice == "4":
            print("👋 Keep creating smarter prompts!")
            break
        else:
            print("⚠️ Invalid choice, try again.")

if __name__ == "__main__":
    main()

