from transformers import pipeline

# Initialize NLP pipelines
summarizer = pipeline("summarization")
generator = pipeline("text-generation")

def analyze_job_description(job_description):
    keywords = summarizer(job_description, max_length=50, min_length=25, do_sample=False)
    return keywords[0]['summary_text']

def generate_content(resume_text, keywords):
    tailored_content = generator(f"Create a resume summary incorporating these keywords: {keywords}", max_length=150)
    return tailored_content[0]['generated_text']

def provide_feedback(resume_text):
    feedback = []
    if len(resume_text) < 200:
        feedback.append("Your resume is too short; consider adding more details.")
    if "responsible" in resume_text:
        feedback.append("Avoid overused phrases like 'responsible for'. Try to use action verbs.")
    return feedback

def personalize_resume(resume_text, keywords):
    for keyword in keywords.split(','):
        if keyword.strip() not in resume_text:
            resume_text += f"\n- {keyword.strip()}"
    return resume_text
def build_resume(job_description, resume_text):
    # Analyze job description
    keywords = analyze_job_description(job_description)

   def build_resume(job_description, resume_text):
    # Analyze job description
    keywords = analyze_job_description(job_description)

    # Generate tailored content
    tailored_content = generate_content(resume_text, keywords)

    # Provide feedback
    feedback = provide_feedback(resume_text)

    # Personalize resume
    personalized_resume = personalize_resume(resume_text, keywords)

    return tailored_content, feedback, personalized_resume
# Sample Input
job_description = """We are looking for a software engineer with experience in Python and machine learning. The ideal candidate should be able to work in a fast-paced environment and collaborate with cross-functional teams."""
resume_text = """Experienced software developer with a strong background in Python and data analysis."""

# Process the input
tailored_content, feedback, personalized_resume = build_resume(job_description, resume_text)

# Display the results
print("Tailored Resume Content:")
print(tailored_content)
print("\nFeedback:")
for item in feedback:
    print("-", item)
print("\nPersonalized Resume:")
print(personalized_resume)
