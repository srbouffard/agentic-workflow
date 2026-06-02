	

| Index | PR009 |
| :---: | :---- |
| **Title** | Jira Issue Types for Engineering |
| **[Status](https://docs.google.com/document/d/1lStJjBGW7lyojgBhxGLUNnliUocYWjAZ1VEbbVduX54/edit?usp=sharing)** | Approved |
| **Authors** | [Maksim Beliaev](mailto:maksim.beliaev@canonical.com) |
| **[Type](https://docs.google.com/document/d/1lStJjBGW7lyojgBhxGLUNnliUocYWjAZ1VEbbVduX54/edit?usp=sharing)** | Process |
| **Created** | Jan 31, 2023 |

# **Required Definitions**

* Issue types distinguish different types of work in the Jira site. They form project structure and hierarchy.  
* Pulse \- Fixed time two week sprint   
* Cycle \- Fixed time 6 months time frame bound to Ubuntu release lifecycle  
* Advanced Roadmap \- Jira Plan, see [official documentation](https://www.atlassian.com/software/jira/guides/advanced-roadmaps/overview#what-are-issue-sources-in-advanced-roadmaps)  
* PM \- Product Manager  
* VP \- Vice President


# **Abstract**

subThe purpose of this specification is to establish a clear definition of issue types to be used across all engineering teams within the company. The defined issue types will serve as a standard framework for engineering projects and will outline how each type of issue relates to the work at hand and the overall company vision.  
That should help in understanding of the work context and show how different products/features relate to each other.

# **Specification**

### **Hierarchy**

![][image1]

Note: in some scenarios work items might not follow the hierarchy. For example, teams may create an Epic to handle Technical Debt resolution, in this case Epic serves more as an issue collector. Thus this type of Epic will not have an associated Objective.  

### **Descriptions**

| Issue Type | Owner | Time scale | Available outside Advanced Roadmap\* | Description |
| ----- | ----- | ----- | :---: | ----- |
| Theme | CEO/VPs | 1+ years | No | Themes represent the company's overall mission and vision for an extended period of time, usually it is several years |
| Objective | VPs/PMs | 1-3 cycles | No | Company objectives are specific, measurable, and time-bound goals that a company sets for itself in order to achieve its overall mission and vision. These objectives typically focus on improving the performance, efficiency, and profitability of the company, while also meeting the needs and expectations of its customers.An objective may include work of several teams on several products in order to achieve it. |
| Epic | Engineering Manager, Director and PMs | Up to a cycle (ideally 1-3 months) | Yes | Represents particular feature implementation towards Objective and relates 1:1 to the roadmap item in [Canonical Product Roadmap](https://docs.google.com/spreadsheets/d/1ykieRvaUVY4fS4ZqGH_jjyQuI6K-wBxgzQoTT2g_3vg/edit#gid=1) In some cases Epic may serve as a collector for items that are not part of the roadmap. One of the examples, a team can create “*Learning 23.10*” Epic, where team members will add all Tasks related to self development during the 23.10 cycle. Another example of Epics being a collector is for support and partnership contracts, where Canonical does not own the Roadmap |
| Story\*\* | Engineering Manager and Engineer | Less than 1 pulse | Yes | An increment towards Epic (feature) implementation. A story is a self-consistent step that provides value on its own. Usually a story should not take more than half of the pulse and definitely fits within a pulse |
| Task\*\* | Engineering Manager and Engineer | Less than 1 pulse | Yes | Tasks have similar size and importance as the user stories, but could be used by teams in order to differentiate functional (stories) and technical (tasks) work types. Tasks could be used to track maintenance work, preparing release notes, etc |
| Spike | Engineering Manager and Engineer | 1 pulse | Yes | Spike is a time-boxed, focused research and development activity that helps teams gain insight or information on a specific aspect of the product. It is a temporary and exploratory effort that is meant to answer a question, explore a new technology, or assess the feasibility of a solution. The goal of a spike is to quickly develop a prototype or a proof-of-concept to validate assumptions and reduce uncertainty, which can then inform the development process and improve decision-making. As an outcome of a spike could be either rejection of the feature/solution or a defined plan for delivering an Epic (Epic must be split into Stories that reflect Epic’s Definition of Done and are required to complete the Epic) Similar to scrum user stories, spikes should be completed within a pulse timeframe. If additional research is required and cannot be completed within a pulse, it should be broken down into multiple spikes. For instance, Spike 1 could involve researching the overall design concept, while Spike 2 and Spike 3 could explore the relationship to Product A and Product B, respectively. |
| Bug | Engineering Manager and Engineer | 1 pulse | Yes | Issue type to log any unexpected behavior that can impact the functionality, usability, and performance of the software |
| Project-Issue | Project Manager / Engineering Manager | Varies | No | Project-Issues represent something which is currently impacting the project, typically in relation to the scope, timeline, resourcing, or quality of deliverables. They can be thought of as risks that have already materialized. In the context of  Engineering, these represent issues impeding the success of delivery of a specific Theme, Objective, Epic, Story or Task for a specific project. Specific information is captured per the existing spec [CEPE012 \- Risk Management for Jira Projects](https://docs.google.com/document/d/1Cpy8Vl7MT9tMI8vqMe91Bf9F9ejynZSjkLJhOGT5nmo/edit?tab=t.0) and this is maintained on an ongoing basis by the Project Manager, in conjunction with the Engineering Manager / Director.  |
| Project-Risk | Project Manager / Engineering Manager | Varies | No | Risks represent challenges which have the potential to negatively impact the project at some point in the future. In the context of  Engineering, these represent risks to the success of delivery of a specific Theme, Objective, Epic, Story or Task for a specific project. Specific information is captured per the existing spec [CEPE013 \- Issue Management for Jira Projects](https://docs.google.com/document/d/10-SYxrvaz7gJwbvHUuWUDgKKaqf_hehB-5oHSg0qOV8/edit?tab=t.0) and this is maintained on an ongoing basis by the Project Manager, in conjunction with the Engineering Manager / Director.  |
| Sub-task | Engineer | Less than story/bug/spike | Yes | **Generally, it is discouraged to use sub-tasks.**  This is the smallest possible work increment. Sub-tasks should be used as a personal to-do list for bug/story/spike. Sub-tasks are not a part of pulses. It is highly recommended to split epics into more Stories rather than using sub-tasks. |

\*Due to the limitations in Jira only 3 levels (levels 3, 4 and 5\) of components are available on the project level. If you want to see other child-parent relations, you have to build a view in Jira Advanced Roadmap. Relations between levels 1, 2 and 3 are done in Jira by applying a “blocked by” link and internal Jira field called “Parent link”.

\*\*Teams can decide if they would like to use both Tasks and Stories in their project or only use one of the types for all the work that they perform.

### **Examples**

* (Theme) Ubuntu Enterprise Platform (Enterprise level operation system)  
  * (Objective) TPM backed Full Disk Encryption  
    * (Epic) \[Foundations\] TPM Passphrase support for subiquity  
      * Spike \- Specification  
      * Spike \- Detect if the kernel snap could be used   
      * Story \- Implement  guided layout  
      * Story \- Add passphrase support  
    * (Epic) \[Desktop\] Updated Installer with Encryption  Options  
      * …  
      * …  
    * (Epic) \[Kernel\] Provide kernel snap for classic Ubuntu  
      * …  
      * …  
* (Theme) First class developer experience   
  * (Objective) 12 factor app deployment   
    * (Epic) \[Starcraft\] Support for a Flask extension for Rockcraft  
      * (Task) Define a common target to build webassets in package.json  
      * (Task) Implement the extensions infrastructure  
      * (Task) Add Flask extension  
    * (Epic) \[Starcraft\] Unify charm YAML files in charmcraft  
      * (Story) Allow for manifest.yaml keys to be used in charmcraft.yaml  
      * (Story) Allow for actions.yaml keys to be used in charmcraft.yaml  
      * (Story) Allow for config.yaml keys to be used in charmcraft.yaml  
      * (Story) Allow for the config.yaml file to be included in charmcraft.yaml  
    * (Epic) \[Starcraft\] Support for a PaaS extension for Charmcraft  
      * (depends on Design and YAML unification)  
    * (Epic) \[IS DevOps\] Design 12 factor app experience   
    * (Epic) \[IS DevOps\] Implementation of Flask base charm  
    * (Epic) \[Web\&Design\] Deploy a web site using 12 factor charm
