# Work Item Fundamentals

In Azure DevOps, a work item represents a discrete unit of work, such as a bug, user story, task, or feature. The concepts of "process" and "progress" in the context of a work item, while related, are distinct:

## Process:

- Refers to the methodology or framework adopted by the team or organization to manage work items. This could be Agile, Scrum, Kanban, or a customized process.
- Defines the different types of work items such as Design, Development, Documentation, 
- Defines the possible types of work required to be performed on a work item. These types of work identify distinct activities such as design, development, testing, documentation, etc. Not all types of work are applicable for every work item. The acceptance criteria should indicate which types of work are needed to meet the definition of done.
- Includes rules, policies, and practices governing the transition of work items between states. For instance, a bug might need to be associate with a completed test task before it can be moved from "Resolved" to "Closed".
- May also define the roles and responsibilities of team members in managing and executing work items.

## Progress:

- Refers to the advancement of a work item through its defined states or stages in the process. For example, moving a user story from "New" to "Active", and eventually to "Closed".
- Is often visualized on boards (like Kanban or Scrum boards) within Azure DevOps, where work items are represented as cards moving through columns that represent their current state.
- Can be measured by metrics such as lead time (the time from work item creation to completion) and cycle time (the time a work item spends in active stages of the process).
- Is affected by various factors including team efficiency, work item complexity, and resource availability.

## Problem Statement

The process should not be seen to be an actual advancement towards completion. It may require a work item to pass through multiple iterations before completion. 

The structured sequence of states or stages a work item passes through (the process) with the actual advancement towards completion (progress)