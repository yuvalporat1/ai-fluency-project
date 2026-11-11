# Project proposal

Degree level: Bachelor’s

## 1. The problem and goal

(Explain the problem you want to solve and why it matters. Describe what you
want to improve, how you will measure improvement, and which limits or
requirements must be respected. A short example can help someone outside your
field understand the problem.)

**The problem and goal in my project:**

In digital chip design, engineers write code (SystemVerilog) to describe hardware.
A constant challenge is optimizing this code for the classic 
"PPA" trade-off: Power, Performance (speed), and Area (silicon cost).
The goal of this project is to automate the structural optimization of digital logic.
I will measure improvement using two metrics: Area (total standard gate count) and 
Delay (the longest logic path, or critical path). The strict requirement is that the
optimized code must remain 100% logically equivalent to the original baseline code for all possible 
inputs. For example, if a baseline design calculates Y = (A * B) + (A * C),
the agent might rewrite it as Y = A * (B + C) to eliminate one massive 
hardware multiplier, vastly reducing the Area while preserving the exact same mathematical output.

## 2. The simulator

(Describe a program or model that could simulate your system and calculate what
happens under different conditions. Explain what inputs it takes and what
results it produces. State whether it can run on your computer and accept inputs
in a text file, such as JSON or YAML. If you have not chosen a simulator yet,
name an option you want to explore and explain what you need to check.)

**The simulator in my project:**

The simulator (ground) is Yosys, an open-source RTL (Register-Transfer Level) synthesis framework.
It takes text-based SystemVerilog files and a short configuration script as inputs.
When executed, Yosys performs two deterministic operations: first, it uses a built-in formal SAT solver to 
mathematically prove whether the newly proposed code is logically equivalent to the baseline code.
Second, it maps the code to basic logic gates and produces a statistical report containing 
the exact gate count (Area) and the maximum logic depth (Delay). Yosys runs entirely locally on my computer 
via a command-line interface, executes in less than a second for small modules, and accepts standard text files.


## 3. The agent's role

(Explain what the language-model-based agent will decide or change, and how it
will use the simulator's results to choose its next action. If known, list the
variables it may change and their allowed ranges. Explain what you expect the
language model to contribute: could a simple program that tries different
options and picks the best result achieve the same goal? Include one question
you still need to answer.)

**The agent in my project:**

The agent acts as a digital logic optimization engineer. It modifies the SystemVerilog source code
to hit specific Area and Delay targets. If the Yosys simulator reports that the Area target was missed,
the agent will rewrite the code to share hardware resources. If the simulator's SAT solver 
reports that the logic was broken, it returns the exact input state (e.g., "Mismatch when A=1, B=0") 
that caused the failure; the agent then uses this to debug and fix its code. A simple brute-force program
cannot achieve this because SystemVerilog is a complex, syntax-heavy language requiring 
semantic understanding of hardware abstractions (like registers, multiplexers, and clock boundaries) to restructure
logic meaningfully. One question I still need to answer is: How heavily do I need to constrain the agent's prompt 
so it does not attempt to change the module's input/output port definitions during its refactoring attempts?

## Explain your project to someone outside your field

(Now describe your whole project again, as if you were explaining it to someone
from another field who has no background in your topic. For example, if you
study chemical engineering, imagine explaining it to a physicist; if you study
aeronautical engineering, imagine explaining it to someone in medicine; if you
study data science, imagine explaining it to a materials engineer. In one or two
short paragraphs, connect all three parts: the problem you want to solve, how
the simulator helps you test ideas, and what the agent will decide or change.
Use everyday language, explain any technical terms you need, and avoid
unexplained abbreviations. Do not simply copy your answers above.)

**My project in simple words:**

Imagine you have a blueprint for a complex physical machine built out of basic Lego blocks.
The machine works perfectly, but it uses far too many blocks (which makes it expensive) and
the path from the start of the machine to the end is too long (which makes it slow). 
The problem is that we need to redesign the blueprint to make the machine smaller and faster,
without breaking its core function. To solve this, I am building an AI agent that acts as a master architect.
The agent writes a new blueprint and hands it to a specialized software 
inspector (the simulator). This inspector mathematically proves whether the new machine does 
the exact same job as the old one, and then counts exactly how many blocks it uses and how fast it runs.
Based on the inspector's report, the AI architect adjusts the blueprint again—perhaps 
combining two gears into one—until it creates a highly efficient machine that passes all of our cost and speed requirements.
