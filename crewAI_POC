from crewai import Agent
from crewai import Crew,Process
from crewai import Task
from crewai_tools import YoutubeChannelSearchTool

OPENAI_API_KEY = ''
OPENAI_MODEL_NAME="gpt-4-0125-preview"

# Forming the tech-focused crew with some enhanced configurations
crew = Crew(
  agents=[blog_researcher, blog_writer],
  tasks=[research_task, write_task],
  process=Process.sequential,  # Optional: Sequential task execution is default
  memory=True,
  cache=True,
  max_rpm=100,
  share_crew=True
)

## start the task execution process with enhanced feedback
result=crew.kickoff(inputs={'topic':'AI VS ML VS DL vs Data Science'})
print(result)

## Sample Code to Create Agents, blog_researcher and blog_writer
blog_researcher=Agent(
    role='Blog Researcher from Youtube Videos',
    goal='get the relevant video transcription for the topic {topic} from the provided Yt channel',
    verbose=True,
    memory=True,
    backstory=("Expert in analyzing videos within AI, Data Science, Machine Learning, and Generative AI, offering valuable insights and actionable recommendations." ),
    tools=[yt_tool],
    allow_delegation=True
)

## creating a senior blog writer agent with YT tool

blog_writer=Agent(
    role='Blog Writer',
    goal='Narrate compelling tech stories about the video {topic} from YT video',
    verbose=True,
    memory=True,
    backstory=(
        "With a talent for simplifying complex topics, you create engaging narratives that both captivate and educate, making new discoveries easily accessible to all."
    ),
    tools=[yt_tool],
    allow_delegation=False
)

## Sample Code to define tasks for the Agents, 1. Research Task and 2. Write Task
research_task = Task(
  description=(
    "Identify the video {topic}. Get detailed information about the video from the channel video."
  ),
  expected_output='A comprehensive 3 paragraphs long report based on the {topic} of video content.',
  tools=[yt_tool],
  agent=blog_researcher,
)

# Writing task with language model configuration
write_task = Task(
  description=("get the info from the youtube channel on the topic {topic}."
  ),
  expected_output='Summarize the info from the youtube channel video on the topic{topic} and create the content for the blog',
  tools=[yt_tool],
  agent=blog_writer,
  async_execution=False,
  output_file='new-blog-post.md'  # Example of output customization
)


# Initialize the tool with a specific Youtube channel handle to target your search
yt_tool = YoutubeChannelSearchTool(youtube_channel_handle='')
