+++
title = "LambdaClass' Culture"
date = 2024-05-01

[taxonomies]
tags=["philosophy"]
+++

# [Lambda’s engineering philosophy](https://blog.lambdaclass.com/lambdas-engineering-philosophy/)

- Observe and Measure
- [The Feynman Technique - YouTube](https://www.youtube.com/watch?v=tkm0TNFzIeg) → Describe the problem or algorithm without technical jargon to facilitate discussion and understanding from various perspectives.
- “The History Written by the Losers”: Those actively involved in activities often lack time for documentation. Consequently, non-practitioners tend to document the findings of others, leading society to believe extensive intellectual and academic work precedes implementation.

## [Principles for success by Ray Dalio](https://www.youtube.com/embed/B9XGUpQZY38)

- Gain alternative perspectives through “other people’s eyes”.

## [Antifragile: Things That Gain from Disorder](https://www.amazon.com/Antifragile-Things-That-Disorder-Incerto/dp/0812979680)

- Aim to be a Phoenix or possibly Hydra to avoid the sword of Damocles.
- “[Hormesis](https://en.wikipedia.org/wiki/Hormesis)”: Small doses of stressors may prove beneficial.
- Consider domain dependence or environmental context.
- “Practice coming before research”: Embrace trial and error.
- “So practice and rediscovery had to be the cause of the interest in Hero’s blueprint, not the other way around.”: Hobbyists can yield disruptive outcomes.
- “The payoff can be so large that you can’t afford not to be in everything.”
- “Planning—it makes the corporation option-blind, as it gets locked into a non-opportunistic course of action.”
- Option = asymmetry + rationality. “The fragile has no option. But the antifragile needs to select what’s best—the best option.”
- “When engaging in tinkering, you incur a lot of small losses, then once in a while you find something rather significant.”
- “(i) Look for optionality; in fact, rank things according to optionality, (ii) preferably with open-ended, not closed-ended, payoffs; (iii) Do not invest in business plans but in people, so look for someone capable of changing six or seven times over his career, or more (an idea that is part of the modus operandi of the venture capitalist Marc Andreessen); one gets immunity from the backfit narratives of the business plan by investing in people. It is simply more robust to do so; (iv) Make sure you are barbelled, whatever that means in your business.”
- Charlatan → An Empiric → Not so intelligent → But trial and error sometimes yield disruptive outcomes.

## [Charity Majors - The Sociotechnical Path to High-Performing Teams](https://www.youtube.com/watch?v=4lLl5B8Oazw)

- “The teams you join will define your career, more than any other single factor.”
- “Being performant is a sign that the majority of your time is spent on things that make life worth living.”
- High-performing teams create exceptional engineers.
- Observability ≠ Monitoring: Observability involves understanding the problem.

## [Citogenesis in science and the importance of real problems](https://lemire.me/blog/2023/06/14/citogenesis-in-science-and-the-importance-of-real-problems/)

- “It is what happens when the truth is determined solely by the literature, not by independent experiments. Everyone assumes, implicitly, that *X*+ is better than *X”*: Test theories and algorithms rather than assuming that one method (*X+*) is inherently superior.
- “A science paper is like a map → […] → we need actual explorers, not just map makers.”

## [How to Make Your Code Reviewer Fall in Love with You](https://mtlynch.io/code-review-love/)

1. Review your code first.
2. Write a clear changelist, provide background references, and consider the reader.
3. Automate simple tasks using linters, GitHub actions/hooks, etc.
4. Attach the code when asking questions. Code comments are acceptable but are inferior to naturally self-documenting code.
5. Changelists that focus on a single change are easier to review and can be reviewed in parallel by teammates.
6. Separate functional and non-functional changes. Beware of unnecessary reformatting that inflates the changelist.
7. Break up large changelists.
8. Artfully solicit missing information: “What would be helpful?”
9. Minimize delay between reviews.

## [The XY Problem](https://xyproblem.info/)

- The issue arises when individuals fixate on what they believe is the solution, neglecting to step back and fully explain the problem.

## [The Biggest Mistake I See Engineers Make](https://web.archive.org/web/20220125174724/https://www.thezbook.com/the-biggest-mistake-i-see-engineers-make/)

- In summary, just as a product benefits from iterative development with users, an engineer’s work improves with iterative collaboration with the product team.

# [The Sunk Cost Fallacy](https://thedecisionlab.com/biases/the-sunk-cost-fallacy/)

## KISS / DTSTTCPW (Do the simplest thing that could possibly work)

- Simplicity does not equate to speed or ease.

## YAGNI → You Aren't Gonna Need It

- If a feature is unnecessary, avoid adding it as it increases deployment time.
- For instance, using lookup tables for error messages simplifies later translations.

## [Design and coding standards](https://github.com/lambdaclass/lambdaclass_hacking_learning_path#design-and-coding-standards)

- [make is the build tool](https://medium.com/@jlouis666/how-to-build-stable-systems-6fe9dcf32fc4#e398).
- Consider [these notes](http://gromnitsky.users.sourceforge.net/articles/notes-for-new-make-users/): "Always remember the power of copying a function, thus breaking a dependency. Fewer dependencies can sometimes be beneficial."
- Code Quality > Speed // Elegance > Speed // Correctness > Speed
- Erlang’s robustness makes it a valuable tool. Other languages should replicate this robustness.
- “A language’s success hinges on its deployment ease. Establish deployment tooling before utilization.”
- Embrace the “Zen of Python”.
- Utilize Git. Use Mermaid for flowcharts, diagrams, and Git graphs.

{% mermaid() %}
gitGraph:
  commit id: "1"
  branch "feat1"
  checkout "feat1"
  commit id: "2"
  commit id: "3"
  checkout main
  commit type: HIGHLIGHT id: "4"
  merge "feat1"
{% end %}
