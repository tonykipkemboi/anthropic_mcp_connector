# Conversation Log - MCP Connector Demo

## Session Start: 2025-08-03

### Prompt 1
```
hey claude, i have a demo i want to build and will need your help along the way. for starters, i have created all the files i'll need to get going and it's in my root. i've also:
- created .venv and activated
- installed my requirements including ipykernel and selecting my kernel (.venv)
- i already did an initial commit as well to my repo

to get started, i'll want you to go through what i have created so far so you're familiar and let me know once you're done so i can continue with further instructions.
```

### Prompt 2
```
before we get started, i'd like you to log all my raw prompts i give you and save it as markdown in a file for documentation and tracing purposes. always update it
```

### Prompt 3
```
cool! also, i have acreated a subagent @agent-claude-api-expert that is an expert at reading, understanding, and answering any python related questions related to Claude API implementation. use this agent whenever you need to clarify or answer a question i ask you about the API implemetation. also for review if i ask you to review my work.
```

### Prompt 4
```
now to my demo idea. 
i'd like to use claude's MCP connector to connect to BrightData's remote stremable-http server and us it to fetch data (i will explain this data shortly).
after data is fetched and saved in JSON's, i'd like to analyze it with claude-opus-4 model and output a report in markdown.
<idea>
so, sometimes you want to connect with people at a company or even figure out info about a company and their employees and who you should connect with.
a good example is if someone is interviewing for a role and they need to connect or find out who in the company has similarities or adjacent roles. this usually involves some manual search on LinkedIn and maybe a few other platforms but hard to tell if you get the right folks or miss some crucial ones. so hence using brightdata through their mcp to parse through all the platforms i list and find the info i need and for opus to synthesize it and give me a report. will talk about the report later.
</idea>
let's brainstorm how this might work
```

### Prompt 5
```
am taking a break but will be right back. yu dont need to know this but helps to log my time spent on this project.
```

### Prompt 6
```
ok, am back online. i'll periodically ask you for help as i code.
```

### Prompt 7
```
look up instructions on how to signup for an account on brightdata and provision an api key that i'll use to connect to their mcp server. i just need a step by step instructions numbered.
```

### Prompt 8
```
i have added a couple of stuff so far and am at this point where the user has to input their details and target company details. i need to make this portion of code below more production ready:
# Enter your information
user_info = {
    "full_name": "Tony Kipkemboi",
    "current_company": "CrewAI",
    "github_username": "tonykipkemboi",
    "linkedin_username": "tonykipkemboi",
    "x_username": "@tonykipkemboi"
}

# Target Company Information
target_info = {
    "company_name": "Anthropic",
    "target_location": "San Francisco, CA",
    "target_role": "Developer Relations Engineer"
}

# Users' social media platforms to scrape data
platforms = [
    f"{label} profile for {user_info[key]}"
    for key, label in [('github_username', 'GitHub'), ('linkedin_username', 'LinkedIn'), ('x_username', 'X')]
    if user_info.get(key)
]

print("\n✅ Information collected successfully!")
print(f"User: {user_info['full_name']}")
print(f"Platforms: {platforms}")
print(f"Target: {target_info['company_name']} - {target_info['target_role']}")
```

### Prompt 9
```
okay, so my input on the notebook is not working well. it's showing running but no input box shows. so for now i'll stick with my initial implementation and change it for production down the road when this is a .py file. no action needed from you for now.
```

### Prompt 10
```
i have some draft prompts i am creating for each of the prompts we'll give claude to use with the mcp server to fetch data. i need you to tap into @agent-claude-api-expert to find out the recommended best practices for prompting shared by Anthropic. find everything about how to create good prompts when using claude. only anthropic docs and articles and nothing else.
<draft>
# Full prompt to fetch user profile data
user_profile_prompt = f"""
    <role>
    You are an expert professional networking researcher with 10+ years of experience in talent acquisition and career analysis. 
    Your specialty is extracting comprehensive professional profiles from public data sources using the BrightData MCP server.
    </role>

    <task>
    Research and extract detailed profile information for {user_info['full_name']} from the specified platforms to support their professional networking strategy.
    </task>

    <platforms>
    {chr(10).join(f"- {platform}" for platform in platforms)}
    </platforms>

    <instructions>
    For each platform found, extract the following information systematically:

    1. PROFESSIONAL BACKGROUND:
    - Current role and company
    - Previous positions and career progression
    - Industry experience and expertise areas
    - Key responsibilities and achievements

    2. SKILLS & EXPERTISE:
    - Technical skills and tools
    - Soft skills and leadership experience
    - Certifications and specializations
    - Projects and notable contributions

    3. EDUCATION:
    - Degrees, universities, graduation years
    - Relevant coursework or academic achievements
    - Professional development and training

    4. INTERESTS & ACTIVITIES:
    - Professional interests and passions
    - Volunteer work and community involvement
    - Hobbies and personal interests
    - Industry participation (conferences, publications)

    5. NETWORKING POTENTIAL:
    - Location and geographic preferences
    - Connection patterns and mutual contacts
    - Communication style and engagement patterns
    - Public contact information (if available)
    </instructions>

    <output_format>
    Return data in structured JSON format with this exact structure:
    {{
    "profile_summary": {{
        "name": "",
        "current_role": "",
        "location": "",
        "industry": ""
    }},
    "platforms": {{
        "github": {{}},
        "linkedin": {{}},
    }},
    "professional_background": [],
    "skills_expertise": [],
    "education": [],
    "interests_activities": [],
    "networking_insights": {{}}
    }}
    </output_format>

    <thinking>
    I need to search each platform systematically and extract comprehensive information that will be valuable for networking analysis and connection-building strategies.
    </thinking>"""

</draft>
fidn ways to optimize it. it's pretty much good but find small marginal improvements. the final output should sdtay the same as a JSON.
```

### Prompt 11
```
let's do the same for the comapny data prompt draft here:
company_profile_prompt = f"""
    <role>
    You are a senior business intelligence analyst specializing in company research and competitive analysis. 
    You have extensive experience in corporate strategy and organizational analysis.
    </role>

    <task>
    Conduct comprehensive research on {target_info['company_name']} to provide strategic insights for a networking and career opportunity analysis.
    </task>

    <instructions>
    Think step-by-step and gather the following information systematically:

    COMPANY FUNDAMENTALS:
    - Official company name, headquarters, founding date
    - Business model, core products/services
    - Mission, vision, and stated values
    - Company size (employees, revenue if public)
    - Geographic presence and key markets

    STRATEGIC POSITION:
    - Industry sector and market position
    - Key competitors and differentiation
    - Recent strategic initiatives and partnerships
    - Growth trajectory and market outlook
    - Major challenges and opportunities

    ORGANIZATIONAL INSIGHTS:
    - Leadership team and key executives
    - Organizational structure and reporting
    - Company culture and work environment
    - Employee value proposition and benefits
    - Diversity and inclusion initiatives

    CURRENT DEVELOPMENTS:
    - Recent news, announcements, and press releases
    - Product launches or strategic changes
    - Hiring trends and expansion plans
    - Financial performance (if public)
    - Industry recognition and awards

    NETWORKING OPPORTUNITIES:
    - Current job openings by department
    - Company events and networking opportunities
    - Employee engagement on professional platforms
    - Corporate social responsibility initiatives

    CRITICAL: You must return ONLY valid JSON, no explanatory text before or after. 
    Start your response with {{ and end with }}.
    </instructions>

    <output_format>
    {{
        "company_overview": {{}},
        "strategic_position": {{}},
        "organizational_insights": {{}},
        "current_developments": {{}},
        "networking_opportunities": {{}},
    }}
    </output_format>

    Return only JSON - no explanatory text."""
```

### Prompt 12
```
and finally the employee data prompt here:
employee_profile_prompt = f"""
    <role>
    You are an expert talent researcher and networking strategist with deep expertise in professional relationship mapping and career transition analysis.
    </role>

    <task>
    Identify and analyze current employees at {target_info['company_name']} who would be valuable networking contacts for someone 
    interested in {target_info['target_role']} positions in {target_info['target_location']}.
    </task>

    <search_criteria>
    - Current employees at {target_info['company_name']}
    - Located in or around {target_info['target_location']}  
    - Working in {target_info['target_role']} or related departments/functions
    - Have public professional profiles (LinkedIn, GitHub, etc.)
    - Actively engaged in professional networking
    </search_criteria>

    <analysis_framework>
    For each employee identified, extract and analyze:

    PROFESSIONAL PROFILE:
    - Full name and current title/role
    - Department and reporting structure
    - Years of experience (total and at company)
    - Career progression and previous roles
    - Key responsibilities and achievements

    BACKGROUND ANALYSIS:
    - Educational background (universities, degrees, years)
    - Previous companies and career path
    - Industry expertise and specializations
    - Technical skills and certifications
    - Geographic career history

    NETWORKING POTENTIAL:
    - Professional interests and passions
    - Industry involvement (conferences, publications, speaking)
    - Social causes and volunteer activities
    - Communication style and engagement level
    - Mutual connections or shared interests
    - Accessibility for networking (LinkedIn activity, etc.)

    CONNECTION STRATEGY:
    - Best approach for initial contact
    - Potential conversation starters
    - Shared experiences or interests
    - Value proposition for connection
    - Follow-up opportunities
    </analysis_framework>

    <examples>
    <example>
    {{
    "name": "Sarah Chen",
    "title": "Senior Product Manager",
    "networking_score": 85,
    "connection_rationale": "Alumni from same university, similar career progression from consulting to product management",
    "conversation_starters": ["Shared consulting background", "Product strategy in fintech"],
    "contact_strategy": "LinkedIn message referencing shared alma mater and interest in product management transition"
    }}
    </example>
    </examples>

    <instructions>
    1. Think step-by-step about the most valuable networking targets
    2. Focus on quality over quantity - aim for 10-15 high-potential contacts
    3. Prioritize employees who are likely to respond positively to networking requests
    4. Consider career level, accessibility, and mutual value potential
    5. Include specific, actionable networking strategies for each person
    </instructions>

    <output_format>
    Return a structured JSON array with detailed profiles, networking scores (1-100), and specific connection strategies for each employee.
    </output_format>

    <thinking>
    I need to identify employees who would be both valuable and accessible for networking, with clear strategies for each connection approach.
    </thinking>"""
```

### Prompt 13
```
am i handling errors sufficiently int his function?
def fetch_data(prompt: str)->str:
    """
    Fetch data from the BrightData MCP server.
    Args:
        prompt: The prompt to send to the MCP server.
    Returns:
        The response from the MCP server.
    """
    try:
        print("🔍 Fetching data...")
        response = anthropic.beta.messages.create(
            model="claude-sonnet-4-20250514",
            max_tokens=4000,
            messages=[{"role": "user", "content": prompt}],
            mcp_servers=[{
                "type": "url",
                "url": BRIGHTDATA_MCP_URL,
                "name": "brightdata-mcp",
                "authorization_token": BRIGHTDATA_TOKEN
            }],
            betas=["mcp-client-2025-04-04"]
        )
        print("✅ Data fetched successfully")
        return response

    except Exception as e:
        print(f"❌ Error fetching user profile data: {e}")
        return None
```

### Prompt 14
```
so i saved the extracted json data for each locally under the @output/ folder. i want to now load the files into variables so i can pass them into the next claude messages call for it to be analyzed and final report generated. i'm sure my code could be more optimal here: def load_json_files() -> tuple[dict, dict, dict]:
    """
    Load the JSON files into variables.
    Returns:
        A tuple containing the user profile data, company profile data, and employee profile data.
    """
    # Load user profile
    with open("output/Tony_Kipkemboi_profile.json", 'r', encoding='utf-8') as f:
        user_data = json.load(f)
    
    # Load company profile
    with open("output/Anthropic_PBC.json", 'r', encoding='utf-8') as f:
        company_data = json.load(f)
    
    # Load employee profiles
    with open("output/Anthropic_PBC_employees.json", 'r', encoding='utf-8') as f:
        employee_data = json.load(f)
    
    return user_data, company_data, employee_data

# Load the data
user_profile_data, company_profile_data, employee_profile_data = load_json_files()
```

### Prompt 15
```
no, i think i want to pass each json fully into the messages prompt as a varibale
```

### Prompt 16
```
now, we're getting to the end here. i need you to research from claude docs on how to write a good prompt for the `messages` section of create() then and only then will you optimize my prompt here: messages=[
        {
            "role": "user",
            "content": f"""I need you to perform a comprehensive strategic networking analysis to help me identify the best connection opportunities at {target_info['company_name']}. 
                        This analysis is critical for my career advancement in {target_info['target_role']} and requires maximum depth.

                        CONTEXT & MOTIVATION:
                        You are analyzing three professional datasets (user profile (me), company profile, and employee profile) to help me strategically network at {target_info['company_name']}. 
                        I need data-driven insights to identify the highest-value connections, understand shared interests, and craft personalized outreach strategies. 
                        This insights will directly impact my ability to build meaningful professional relationships and advance my career in {target_info['target_role']}.
                        Do NOT use code execution or write code to generate the analysis, use the data provided to generate the analysis.

                        DATASETS TO USE:
                        === USER PROFILE DATA (ME)===
                        {json.dumps(user_profile_data, indent=2)}

                        === COMPANY PROFILE DATA ===
                        {json.dumps(company_profile_data, indent=2)}

                        === EMPLOYEE PROFILES DATA ===
                        {json.dumps(employee_profile_data, indent=2)}

                        RESPONSE FORMAT:
                        - Structure your analysis for clear organization and readability as follows:
                        - Provide a brief overview of key findings and top recommendations
                        - Detailed compatibility analysis based on the data provided.
                        - Rank the profiles of the employees I should connect with, including specific commonalities and why they are a good fit.
                        - Personalized connection messages for each top target (under 200 characters each) that are tailored to the target and match my personality and tone based on my profile and not cringeworthy.
                        - Actionable networking strategies, timing, and tactical advice including a list of best questions to ask about the company and the target employee based on the data provided.
                        - Focus on delivering maximum value through deep insights, practical recommendations, and ready-to-use outreach materials.

                        The final output should be in a well formatted markdown format with all the information in a structured way and no markdown tags like <>, <>, '```', etc.
                        It should be a final professional report.
                        """
        }
    ]
```

---
*Last updated: 2025-08-03*