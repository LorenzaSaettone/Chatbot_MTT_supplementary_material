# Supplementary Material

**Paper:** Combining VR and Chatbot-Based Training to Improve Metaphorical Understanding  
**Authors:** Lorenza Saettone, Carmine Tommaso Recchiuto, Hui-Yin Wu  
**Venue:** ICEC 2026

---

## A. Chatbot Prompting Structure

The chatbot backend is a Flask server communicating with GPT-4o via the OpenAI API. Participant responses are evaluated in two distinct phases, each with its own prompt.

### A.1 Similarity Phase (temperature = 0)

**System prompt:**

```
You are a language model that speaks Italian and helps users train with
metaphorical comprehension. The user is asked to find meaningful concepts
or properties that associate these subjects: {concept}.
Your task is to respond only with 0 for no and 1 for yes.
Respond with 1 only if the percentage of semantic correlation is over
90 percent with the list of correct answers.
```

**User prompt:**

```
TASK: Determine if the user's answer '{user_input}' is a correct
metaphorical similarity among these concept '{concept}', matching any
correct answers I provide you in list {correct_answers}.
I also provide you possible wrong answers in another list {wrong_answers}.
Respond only with 0 for no and 1 for yes, and answer 1 only if the
percentage of semantic correlation is over 90 percent with the list of
correct expected answers: several answers and ways of answering are possible.

OUTPUT: You must evaluate the user answer '{user_input}'. Return 0 if
the user responded incorrectly, or doesn't know the answer; answer 1
if they respond correctly in identifying the correct characteristics
to explain the metaphor in the training.

CONTEXT: This metaphorical training is a test in which the user is asked
to identify the right properties shared among these concepts '{concept}',
to determine the meaning of a metaphor.

Examples of possible correct answers: {correct_answers}.
Examples of possible incorrect answers: {wrong_answers}.

LONG-TERM EFFECT: Through this test, the user must learn to find the
characteristics to understand the metaphor without interpreting it
literally or responding off-topic.
```

### A.2 Metaphor Formulation Phase (temperature = 0.5)

**System prompt:**

```
You are a LLM that speaks Italian and calculates conceptual similarities
between the actual answer and hypothetical ones, in given lists that
exemplify right answers and wrong ones. Only answer 0 for no, 1 for yes,
if the user answers as expected AND ARE IN THE FORM OF ANALOGIES,
METAPHORS, SIMILES. So consider two parameters, ONLY answer with number
values: 1 for yes or 0 for no. Answer 1 only if the percentage of
semantic correlation calculated is high, over 90 percent, and if the
user's answer is an explicit metaphor, analogy, or comparison.
```

**User prompt:**

```
ONLY answer 0 for no or 1 for yes to these 2 questions:
Is this answer '{user_input}' a comparison or a metaphor?
And is it related to all these sentences in the list: {correct_answers}?
If not, answer 0.
Here are a few EXAMPLES FOR 0: If '{user_input}' is related to
{wrong_answers}, answer 0, NO.
```

### A.3 Hint Delivery

On incorrect responses, the system delivers three progressive hints (hardcoded per image pair per session):

- **Count = 2 → Guiding question:** directs the participant toward the relevant property without naming it.  
  *Example (fishing net / spider web):* "Does it seem like there is something similar in the form or in the feeling they evoke?"

- **Count = 3 → Misleading question:** built around a contrary property to force reconsideration.  
  *Example:* "Think about whether you can easily escape from the net? So, try to find the similarity between the two elements."

- **Count = 4 → Solution:** states the shared property directly.  
  *Example:* "Both are about being trapped in a net. Now try to formulate a clear and simple metaphor."

### A.4 Example Dictionary Entry (Session 1, Image 0: Fishing Net / Spider Web)

```json
{
  "somiglianze_0": [
    "trapped", "immobilized", "the net", "on the verge of death",
    "stuck in a hostile context", "depend on an external agent to be freed",
    "the fish cannot escape the net like the fly from the web",
    "the net and the web have a similar function", "the shape",
    "net-like shape used to catch prey",
    "caught in a mechanism stronger than them"
  ],
  "allucinazioni_0": [
    "the fish is hungry because the spider caught the fly"
  ],
  "concetto_0": ["fish in the net and fly in the spider web"],
  "metafora_0": "The fish in the net is like a fly in the web: both are
    trapped in a net that immobilizes and makes them vulnerable."
}
```

### A.5 Example Dictionary Entry (Session 1, Image 5: Elderly Man / Ancient Tree)

```json
{
  "somiglianze_5": [
    "long-lived, tough, resistant, wrinkled, wise, rooted in traditions,
     slow but relentless",
    "old age and antiquity", "gnarled",
    "one leans on a cane like the other on its trunk",
    "longevity", "toughness", "slow",
    "deep roots and anchorage in tradition",
    "the bark is gnarled like aged skin is wrinkled and bumpy",
    "both seem made of bark and wrinkles",
    "the metaphor is about old age, resistance, wisdom",
    "both are in the winter of their existence"
  ],
  "allucinazioni_5": [
    "grandfather, old man, elderly person",
    "farmer", "they are natural entities", "they are made by God",
    "the old man and the sea", "the old man planted an olive tree",
    "I don't know", "they are trees", "tree, plant",
    "life", "existence", "intelligence", "brute force"
  ],
  "concetto_5": ["elderly man and century-old tree"],
  "metafora_5": "The old man is an olive tree: this metaphor means that
    both are long-lived, tough, resistant, wrinkled, wise, rooted in
    traditions, slow but relentless."
}
```

---

## B. PMM Items

The twelve Physical and Mental Metaphors (Del Sette et al., 2020) used for pre- and post-training assessment. The same items were administered in Italian and French; English translations are provided here.

**Pre-training (6 items)**

1. The footballer is an arrow
2. Mummy is a candy
3. Soldiers are lions
4. The chef is a barrel
5. Dancers are butterflies
6. Grandma is a column

**Post-training (6 items)**

1. My brother is a skyscraper
2. Players are elephants
3. The teacher is an icicle
4. Daddy is a volcano
5. Climbers are squirrels
6. That pupil is a sponge

### B.1 PMM Scoring Examples

Each PMM response is scored 0 (incorrect), 1 (partially correct), or 2 (fully correct) by two independent raters, following the criteria defined by Del Sette et al. (2020). A score of 2 requires identifying the property that is most salient in the interaction between the two domains of the metaphor; a score of 1 is assigned when the response captures a plausible but non-target property; a score of 0 is assigned when the response is literal, off-topic, or incorrect.

| Metaphor | Response | Score | Rationale |
|---|---|---|---|
| The chef is a barrel | "The chef holds all the secrets of cooking" | 0 | Confuses the idiom "in a barrel of iron" (i.e., safe/protected) with "barrel" as a physical descriptor; the relevant shared property is physical roundness/large size |
| The footballer is an arrow | "The footballer kicks the ball precisely like an arrow" | 0 | Misattributes the metaphor to the ball rather than the footballer; the metaphor states that the footballer *is* an arrow, not that the ball behaves like one |
| The footballer is an arrow | "The footballer is precise" | 1 | Identifies a plausible property of arrows, but the target similarity is speed/directionality |
| The footballer is an arrow | "The footballer is fast and direct, like an arrow" | 2 | Correctly identifies the shared property: speed and directionality |
| That pupil is a sponge | "The pupil drinks" | 1 | Captures a partial mapping (absorbing liquid); the target property in the pupil–sponge interaction is absorbing knowledge |
| That pupil is a sponge | "The pupil absorbs knowledge easily, like a sponge absorbs water" | 2 | Correctly identifies the shared property emerging from the interaction between the two domains |

---

## C. VR Scenario Screenshots

The eight VR scenarios corresponding to MTT training items. Each scenario translates a visual metaphor into an interactive, game-like quest.

### Scenarios 1–4

![VR scenarios 1–4](from%201%20to%204%20screenshots%20vr.png)

1. **Spinning Top = Ballerina** — A child's room with toys on the floor. The participant must find a ballerina among the toys and place her on a shelf-stage framed by a red curtain. *Distractor:* a ballerina drawing in a book.
2. **Grandpa = Ancient Tree** — An elderly man's house with a piano. The participant must find the "roots" (a vintage photo) and place them under the "trunk" (a wooden cane). *Distractor:* a real tree visible outside the window.
3. **Oasis = Beacon** — An oasis in the desert beside a collapsed man. The participant must fill the "beacon" (an empty bottle) with light (oasis water) and bring it to the "ship in trouble" (the collapsed person). *No distractor.* (Distractors are literal-meaning objects; other objects present in the scene, such as a treasure or a book, are off-topic items, not literal distractors.)
4. **Angry Person = Bomb** — A crowded bus stop near a park. The participant must defuse the metaphorical "bomb" by offering a flower to the angry person. *Distractor:* a real bomb.

### Scenarios 5–8

![VR scenarios 5–8](from%204%20to%208%20screenshots%20vr.png)

5. **Windows = Eyes** — A summer courtyard in front of a house. The participant must "open someone's eyes" by ringing the doorbell and opening the house's window-"eyes." *Distractor:* a figure with closed eyes on a bench.
6. **Xmas Decorations = Jewels** — A Christmas living room with fire and radio music. The participant must "dress" the Christmas tree with jewels (earrings and baubles). *No distractor.*
7. **Farmer = Rooster** — A rural setting with a ranch and farm animals. The participant must "feed the rooster," i.e., photograph the vain farmer showing off his muscles. *Distractor:* a real rooster and grain.
8. **Bridal Veil = Fog** — A Gothic square with church, gargoyles, boutique, and fog. The participant must dissipate the fog, i.e., cut the veil on a mannequin. *Distractor:* real fog.
