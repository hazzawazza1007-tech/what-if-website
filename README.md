# what-if-website
The official WHAT IF?  website

<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>WHAT IF?</title>
</head>
<body>
  <main>
    <h1>WHAT IF?</h1>
    <h2 id="q">Today's question</h2>
    <p id="intro"></p>
    <p id="ex"></p>
    <button id="share" type="button">Share this scenario</button>

    <section class="about">
      <h2>About</h2>
      <p>Explore one extraordinary scenario each day.</p>
    </section>

    <p id="toast" aria-live="polite"></p>
  </main>

  <!-- Copy the existing <script> ... </script> block from README.md here. -->
</body>
</html>
<script>
const scenarios = [
  {
    question: "What if a record-sized tsunami hit?",
    intro: "Imagine an enormous wall of water approaching a populated coastline. What would happen if a record-breaking tsunami reached land?",
    explanation: "A massive tsunami could flood coastal communities, destroy buildings, damage roads and overwhelm emergency services. The impact would depend on the wave's height, speed and the shape of the coastline."
  },
  {
    question: "What if there was a meteor shower?",
    intro: "Imagine the night sky filling with streaks of light, followed by unusually large space rocks reaching the ground.",
    explanation: "Most meteors burn up harmlessly in the atmosphere. Exceptionally large fragments could survive the journey and cause explosions, fires and serious damage if they struck populated areas."
  },
  {
    question: "What if the Moon disappeared?",
    intro: "Imagine looking up at night and discovering that the Moon had suddenly vanished.",
    explanation: "Earth would lose its main source of tides, making ocean patterns change dramatically over time. Nights would also become much darker, and Earth's long-term stability could be affected."
  },
  {
    question: "What if Earth stopped spinning?",
    intro: "Imagine the ground beneath your feet suddenly behaving as though the planet had stopped rotating.",
    explanation: "A sudden stop would cause enormous movement of the atmosphere and oceans. The consequences would be devastating, with extreme winds, flooding and widespread destruction."
  },
  {
    question: "What if every volcano erupted at once?",
    intro: "Imagine volcanoes around the world erupting at the same time, filling the sky with ash and smoke.",
    explanation: "Large eruptions could disrupt air travel, damage crops, affect breathing and cool parts of the planet by blocking sunlight. The actual effects would depend on the size of each eruption."
  },
  {
    question: "What if the Sun became twice as bright?",
    intro: "Imagine stepping outside and discovering that sunlight had suddenly become dramatically more intense.",
    explanation: "Earth would receive far more energy, raising temperatures and putting pressure on ecosystems, oceans and agriculture. The consequences would depend on how quickly the change happened."
  }
];

// Use Perth time so everyone sees the same daily scenario.
const parts = new Intl.DateTimeFormat("en", {
  timeZone: "Australia/Perth",
  year: "numeric",
  month: "2-digit",
  day: "2-digit"
}).formatToParts(new Date());

const dateKey =
  parts.find(p => p.type === "year").value + "-" +
  parts.find(p => p.type === "month").value + "-" +
  parts.find(p => p.type === "day").value;

// Create a repeatable number from today's date.
let hash = 0;
for (const character of dateKey) {
  hash = (hash * 31 + character.charCodeAt(0)) >>> 0;
}

// Everyone gets the same scenario on the same Perth date.
const today = scenarios[hash % scenarios.length];

// Update the website.
document.getElementById("q").textContent = today.question;
document.getElementById("intro").textContent = today.intro;
document.getElementById("ex").textContent = today.explanation;
document.title = "WHAT IF? — " + today.question;

document.querySelector(".about p").textContent =
  "Each day, WHAT IF? explores one extraordinary scenario through a visual and a short, clear explanation. Today's question: " +
  today.question;

// Update the share button to share today's scenario.
document.getElementById("share").onclick = async () => {
  const text = today.question + " " + today.intro + " Explore the scenario on WHAT IF?!";

  try {
    if (navigator.share) {
      await navigator.share({ title: "WHAT IF?", text });
    } else {
      await navigator.clipboard.writeText(text);
      const toast = document.getElementById("toast");
      toast.textContent = "Scenario copied!";
      toast.classList.add("show");
      setTimeout(() => toast.classList.remove("show"), 2000);
    }
  } catch (error) {}
};
// Reload shortly after midnight in Perth.
const perthNow = new Date(Date.now() + 8 * 60 * 60 * 1000);
const nextMidnight = Date.UTC(
  perthNow.getUTCFullYear(),
  perthNow.getUTCMonth(),
  perthNow.getUTCDate() + 1
) - 8 * 60 * 60 * 1000;

setTimeout(() => location.reload(), nextMidnight - Date.now() + 1000);
</script>
