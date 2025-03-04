<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Human Centred Design Dictionary</title>
  <!-- Include D3.js from CDN -->
  <script src="https://d3js.org/d3.v7.min.js"></script>
  <style>
    /* General styling */
    body {
      margin: 0;
      font-family: Arial, sans-serif;
      background-color: #e0e0e0;
      /* Circular-themed background using a repeating radial pattern */
      background-image: repeating-radial-gradient(circle at center, rgba(255,255,255,0.5), rgba(255,255,255,0.5) 2px, transparent 2px, transparent 40px);
      color: #333;
      overflow: hidden;
    }
    header {
      position: fixed;
      top: 0;
      width: 100%;
      padding: 10px 20px;
      background: rgba(255,255,255,0.95);
      box-shadow: 0 2px 4px rgba(0,0,0,0.1);
      display: flex;
      justify-content: space-between;
      align-items: center;
      z-index: 100;
    }
    header .logo {
      font-size: 24px;
      font-weight: bold;
      color: #333;
    }
    header nav a {
      margin: 0 10px;
      text-decoration: none;
      color: #333;
      font-weight: 500;
      cursor: pointer;
    }
    .search-bar {
      width: 40%;
      display: flex;
      align-items: center;
    }
    .search-bar input {
      flex: 1;
      padding: 8px 12px;
      font-size: 16px;
      border: 1px solid #ccc;
      border-radius: 20px;
      outline: none;
    }
    .search-bar button {
      margin-left: 5px;
      padding: 8px 12px;
      font-size: 16px;
      border: none;
      background-color: #69b3a2;
      color: #fff;
      border-radius: 20px;
      cursor: pointer;
    }
    /* Tooltip styling */
    .tooltip {
      position: absolute;
      text-align: center;
      padding: 8px;
      font: 12px sans-serif;
      background: rgba(0, 0, 0, 0.7);
      color: #fff;
      border-radius: 4px;
      pointer-events: none;
    }
    /* Modal styling */
    .modal {
      display: none;
      position: fixed;
      z-index: 200;
      left: 0;
      top: 0;
      width: 100%;
      height: 100%;
      overflow: auto;
      background-color: rgba(0,0,0,0.4);
    }
    .modal-content {
      background-color: #fff;
      margin: 15% auto;
      padding: 20px;
      border: 1px solid #888;
      width: 80%;
      max-width: 500px;
      text-align: center;
      border-radius: 10px;
    }
    .close {
      color: #aaa;
      float: right;
      font-size: 28px;
      font-weight: bold;
      cursor: pointer;
    }
    .close:hover,
    .close:focus {
      color: #000;
      text-decoration: none;
      cursor: pointer;
    }
    /* Main view containers */
    main {
      margin-top: 60px; /* space for header */
      height: calc(100vh - 60px);
      overflow-y: auto;
    }
    #networkView, #termView {
      display: none;
      height: 100%;
    }
    #networkView.active, #termView.active {
      display: block;
    }
    /* Network view styling */
    #network {
      width: 100%;
      height: 60%;
    }
    .section {
      padding: 20px;
      background: #fff;
      margin: 10px;
      border-radius: 5px;
    }
    /* Term detail view styling */
    #termView header {
      background: #69b3a2;
      color: #fff;
      padding: 10px 20px;
      text-align: center;
    }
    #termView nav a {
      color: #fff;
      text-decoration: none;
      margin: 0 10px;
    }
    #termContent {
      max-width: 800px;
      margin: 20px auto;
      padding: 20px;
      background: #fff;
    }
    #termContent section {
      margin-bottom: 40px;
    }
    #termContent h1, #termContent h2 {
      margin-top: 0;
    }
    .visual-aids img {
      max-width: 100%;
      height: auto;
      display: block;
      margin: 10px 0;
      border: 2px dashed #ccc; /* Placeholder border */
    }
    .quiz, .comments, .accessibility-controls {
      border: 1px solid #ccc;
      padding: 15px;
      border-radius: 5px;
      margin-bottom: 20px;
    }
    .accessibility-controls button {
      margin-right: 10px;
      padding: 8px;
      border: none;
      border-radius: 4px;
      cursor: pointer;
    }
    #backButton {
      display: block;
      margin: 10px auto;
      padding: 10px 20px;
      font-size: 16px;
      background: #69b3a2;
      color: #fff;
      border: none;
      border-radius: 5px;
      cursor: pointer;
    }
    footer {
      text-align: center;
      padding: 10px;
      background: #eee;
    }
  </style>
</head>
<body>
  <!-- Fixed Header -->
  <header>
    <div class="logo">HCD Dictionary by Vedant Kabra</div>
    <div class="search-bar">
      <input type="text" id="search" placeholder="Search HCD terms...">
      <button id="searchArrow">→</button>
    </div>
    <nav>
      <a href="#about" onclick="scrollToSection('about')">About</a>
      <a href="#contribute" onclick="scrollToSection('contribute')">Contribute</a>
    </nav>
  </header>

  <main>
    <!-- Network View -->
    <div id="networkView" class="active">
      <!-- SVG container for interactive network -->
      <svg id="network"></svg>
      <!-- About Section -->
      <div class="section" id="about">
        <h2>About</h2>
        <p>This website is dedicated to Human-Centered Design (HCD) terminologies. It is designed as an interactive dictionary and hub for learning and applying HCD principles.</p>
      </div>
      <!-- Contribute Section -->
      <div class="section" id="contribute">
        <h2>Contribute</h2>
        <p>Have a term or an improved definition? Please contribute and help keep this dictionary growing with the latest insights in HCD!</p>
      </div>
    </div>

    <!-- Term Detail View -->
    <div id="termView">
      <header>
        <h1 id="termTitle">Term Title</h1>
        <nav>
          <a href="#" id="termNavHome" onclick="goBack()">Home</a>
          <a href="#definition">Definition</a>
          <a href="#examples">Examples</a>
          <a href="#quiz">Quiz</a>
          <a href="#discussion">Discussion</a>
        </nav>
      </header>
      <div id="termContent">
        <!-- Definition & Detailed Explanation -->
        <section id="definition">
          <h2>Definition & Explanation</h2>
          <p id="termDefinition"><strong>Term</strong> detailed definition goes here.</p>
          <p id="termExplanation">Detailed explanation, examples, and insights about the term.</p>
        </section>
        <!-- Visual Aids -->
        <section class="visual-aids">
          <h2>Visual Aids</h2>
          <p>The diagram below illustrates the concept:</p>
          <!-- Diagram image (source updated per term) -->
          <img id="termDiagram" src="https://via.placeholder.com/750x300?text=Placeholder+Diagram" alt="Placeholder diagram illustrating the term">
          <p>Watch this video for a deeper explanation:</p>
          <!-- YouTube embed example -->
          <div style="position:relative;padding-bottom:56.25%;height:0;overflow:hidden;max-width:100%;">
            <iframe id="termVideo" src="" style="position:absolute;top:0;left:0;width:100%;height:100%;" frameborder="0" allowfullscreen title="Video explanation"></iframe>
          </div>
        </section>
        <!-- Related Terms -->
        <section id="related">
          <h2>Related Terms</h2>
          <ul id="relatedTerms">
            <!-- Dynamically filled -->
          </ul>
        </section>
        <!-- Real-World Examples & Case Studies -->
        <section id="examples">
          <h2>Real-World Examples & Case Studies</h2>
          <p id="caseStudiesIntro">Organizations have applied this concept in various innovative ways:</p>
          <ul id="caseStudies">
            <!-- Dynamically filled with full-text articles -->
          </ul>
        </section>
        <!-- Interactive Quiz -->
        <section id="quiz" class="quiz">
          <h2>Interactive Quiz</h2>
          <form id="quizForm">
            <!-- Quiz content dynamically populated -->
          </form>
          <p id="quizFeedback"></p>
        </section>
        <!-- User Contributions & Discussion -->
        <section id="discussion" class="comments">
          <h2>User Contributions & Discussion</h2>
          <p>Share your insights and experiences:</p>
          <textarea id="commentBox" rows="4" style="width: 100%;" placeholder="Enter your comment here"></textarea>
          <br>
          <button onclick="submitComment()">Submit Comment</button>
          <div id="commentsSection">
            <h3>Comments:</h3>
            <ul id="commentsList">
              <!-- Comments will appear here -->
            </ul>
          </div>
        </section>
        <!-- Accessibility Controls -->
        <section class="accessibility-controls">
          <h2>Accessibility Options</h2>
          <button onclick="changeFontSize('increase')">Increase Font Size</button>
          <button onclick="changeFontSize('decrease')">Decrease Font Size</button>
          <button onclick="toggleContrast()">Toggle High Contrast</button>
        </section>
        <!-- Back Button -->
        <button id="backButton" onclick="goBack()">Back to Network</button>
      </div>
    </div>
  </main>

  <footer>
    <p>&copy; 2025 Human Centred Design Dictionary. All rights reserved.</p>
  </footer>

  <script>
    /* ------------------------- Utility Functions ------------------------- */
    function scrollToSection(id) {
      const el = document.getElementById(id);
      if (el) {
        el.scrollIntoView({ behavior: "smooth" });
      }
    }

    /* ------------------------- Network View (D3.js) ------------------------- */
    let width = window.innerWidth;
    let height = window.innerHeight * 0.6; // 60% for network SVG

    // Always create our container and groups from it.
    const svg = d3.select("#network")
                  .attr("width", width)
                  .attr("height", height);
    const container = svg.append("g").attr("class", "container");
    const link = container.append("g").attr("class", "links");
    const node = container.append("g").attr("class", "nodes");
    const labels = container.append("g").attr("class", "labels");

    // Expanded nodes with additional HCD terms.
    const nodes = [
      { id: "Empathize", definition: "Understanding user needs and emotions." },
      { id: "Define", definition: "Framing the problem based on insights." },
      { id: "Ideate", definition: "Generating a wide range of ideas." },
      { id: "Prototype", definition: "Building tangible representations of ideas." },
      { id: "Test", definition: "Evaluating prototypes with users." },
      { id: "User Research", definition: "Collecting data on user behavior." },
      { id: "Journey Mapping", definition: "Visualizing the user experience over time." },
      { id: "Persona", definition: "Creating representative user profiles." },
      { id: "Usability", definition: "Assessing ease of use in design." },
      { id: "Data Driven Design", definition: "Using data to guide design decisions." },
      { id: "Qualitative", definition: "Focusing on rich, user experience insights." },
      { id: "Pivot", definition: "A strategic shift based on feedback." },
      { id: "Expansion", definition: "Broadening the design scope." },
      { id: "Customer Segment", definition: "Grouping users by shared characteristics." },
      { id: "Human Centred Design", definition: "Design that prioritizes user needs." },
      { id: "System Design", definition: "Creating interconnected, holistic systems." },
      { id: "ESGs", definition: "Integrating environmental, social, and governance factors." },
      { id: "Greenwashing", definition: "Misleading claims about environmental benefits." },
      { id: "Strategy", definition: "Long-term planning aligning design with goals." },
      { id: "Product Centred Design", definition: "Emphasizing product features over user experience." },
      { id: "User Centred Design", definition: "Placing the user at the heart of design." },
      { id: "Methodology", definition: "A structured approach to the design process." },
      { id: "Principles", definition: "Core guidelines that shape design decisions." },
      { id: "Empathy", definition: "Understanding and sharing user emotions." },
      { id: "Iterate", definition: "Continuously refining design solutions." }
    ];

    // Example links between some terms (adjust as needed)
    const links = [
      { source: "Empathize", target: "User Research" },
      { source: "Empathize", target: "Qualitative" },
      { source: "Define", target: "Strategy" },
      { source: "Ideate", target: "Pivot" },
      { source: "Prototype", target: "Iterate" },
      { source: "Test", target: "User Centred Design" },
      { source: "Journey Mapping", target: "Customer Segment" },
      { source: "Data Driven Design", target: "Methodology" },
      { source: "Human Centred Design", target: "Empathy" },
      { source: "System Design", target: "Principles" },
      { source: "ESGs", target: "Greenwashing" },
      { source: "Product Centred Design", target: "User Centred Design" }
    ];

    const simulation = d3.forceSimulation(nodes)
                         .force("link", d3.forceLink(links).id(d => d.id).distance(150))
                         .force("charge", d3.forceManyBody().strength(-400))
                         .force("center", d3.forceCenter(width / 2, height / 2));

    link.selectAll("line")
        .data(links)
        .enter().append("line")
        .attr("stroke-width", 2)
        .attr("stroke", "#aaa");

    node.selectAll("circle")
        .data(nodes)
        .enter().append("circle")
        .attr("r", 75)  // Radius set to 75 pixels
        .attr("fill", "#69b3a2")
        .call(drag(simulation));

    labels.selectAll("text")
          .data(nodes)
          .enter().append("text")
          .attr("dy", ".35em")
          .attr("text-anchor", "middle")
          .text(d => d.id)
          .attr("fill", "#fff")
          .attr("pointer-events", "none")
          .attr("font-size", "14px");

    node.selectAll("circle")
        .on("mouseover", (event, d) => {
          tooltip.transition().duration(200).style("opacity", 0.9);
          tooltip.html("<strong>" + d.id + "</strong><br/>" + d.definition)
                 .style("left", (event.pageX + 10) + "px")
                 .style("top", (event.pageY - 28) + "px");
        })
        .on("mouseout", () => {
          tooltip.transition().duration(500).style("opacity", 0);
        })
        .on("click", (event, d) => {
          zoomToNode(d);
          showModal(d);
        });

    simulation.on("tick", () => {
      link.selectAll("line")
          .attr("x1", d => d.source.x)
          .attr("y1", d => d.source.y)
          .attr("x2", d => d.target.x)
          .attr("y2", d => d.target.y);
      node.selectAll("circle")
          .attr("cx", d => d.x)
          .attr("cy", d => d.y);
      labels.selectAll("text")
            .attr("x", d => d.x)
            .attr("y", d => d.y);
    });

    function drag(simulation) {
      function dragstarted(event, d) {
        if (!event.active) simulation.alphaTarget(0.3).restart();
        d.fx = d.x;
        d.fy = d.y;
      }
      function dragged(event, d) {
        d.fx = event.x;
        d.fy = event.y;
      }
      function dragended(event, d) {
        if (!event.active) simulation.alphaTarget(0);
        d.fx = null;
        d.fy = null;
      }
      return d3.drag()
               .on("start", dragstarted)
               .on("drag", dragged)
               .on("end", dragended);
    }

    const zoom = d3.zoom().on("zoom", (event) => {
      container.attr("transform", event.transform);
    });
    svg.call(zoom);

    function zoomToNode(nodeData) {
      const scale = 2;
      const translateX = width / 2 - nodeData.x * scale;
      const translateY = height / 2 - nodeData.y * scale;
      svg.transition().duration(750).call(
        zoom.transform,
        d3.zoomIdentity.translate(translateX, translateY).scale(scale)
      );
    }

    /* ---------------------- Modal Functionality ---------------------- */
    const modal = d3.select("body").append("div")
                    .attr("id", "modal")
                    .attr("class", "modal")
                    .style("display", "none");
    const modalContent = modal.append("div").attr("class", "modal-content");
    modalContent.append("span")
                .attr("id", "closeModal")
                .attr("class", "close")
                .html("&times;");
    modalContent.append("h2").attr("id", "modalTitle").text("Term Title");
    modalContent.append("p").attr("id", "modalDefinition").text("Term Definition");
    modalContent.append("button")
                .attr("id", "learnMore")
                .text("Learn More")
                .on("click", function() {
                  loadTerm(currentNode.id);
                });

    let currentNode = null;
    function showModal(nodeData) {
      currentNode = nodeData;
      d3.select("#modalTitle").text(nodeData.id);
      d3.select("#modalDefinition").text(nodeData.definition);
      modal.style("display", "block");
    }
    d3.select("#closeModal").on("click", () => {
      modal.style("display", "none");
    });

    /* ---------------------- Search Functionality ---------------------- */
    const searchInput = d3.select("#search");
    const searchArrow = d3.select("#searchArrow");
    function searchAndZoom() {
      const query = searchInput.property("value").toLowerCase();
      const found = nodes.find(n => n.id.toLowerCase() === query);
      if (found) {
        zoomToNode(found);
        showModal(found);
      } else {
        alert("Term not found.");
      }
    }
    searchArrow.on("click", searchAndZoom);
    searchInput.on("keydown", (event) => {
      if (event.key === "Enter") {
        event.preventDefault();
        searchAndZoom();
      }
    });

    function fitGraph() {
      const containerBBox = container.node().getBBox();
      const scale = Math.min(width / containerBBox.width, height / containerBBox.height) * 0.9;
      const translate = [
        (width - containerBBox.width * scale) / 2 - containerBBox.x * scale,
        (height - containerBBox.height * scale) / 2 - containerBBox.y * scale
      ];
      container.transition().duration(750)
               .attr("transform", "translate(" + translate + ") scale(" + scale + ")");
    }
    setTimeout(fitGraph, 3000);
    window.addEventListener('resize', () => {
      width = window.innerWidth;
      height = window.innerHeight * 0.6;
      svg.attr("width", width).attr("height", height);
      simulation.force("center", d3.forceCenter(width / 2, height / 2));
      simulation.alpha(0.3).restart();
      setTimeout(fitGraph, 1000);
    });

    /* ---------------------- Term Detail Data & Functionality ---------------------- */
    // Diagram URL assignments per group:
    // Group 1: Empathize, Define, Ideate, Prototype, Test
    const group1Diagram = "https://stock.adobe.com/in/images/infographic-design-thinking-process-empathise-define-ideate-prototype-and-test-in-five-steps-with-circle-timeline-and-paper-style-the-illustration-for-develop-innovative-technology/417113436";
    // Group 2: User Research
    const group2Diagram = "https://xtensio.com/persona-based-customer-journey-map-template/";
    // Group 3: Journey Mapping
    const group3Diagram = "https://www.uxtweak.com/user-journey-map/examples/";
    // Group 4: Persona
    const group4Diagram = "https://maze.co/guides/user-personas/persona-mapping/";
    // Group 5: Data Driven Design, Usability
    const group5Diagram = "https://uxbooth.com/articles/ux101-data-driven-user-journeys/";
    // Group 6: Qualitative, Pivot, Expansion, Customer Segment
    const group6Diagram = "https://www.uxmatters.com/mt/archives/2020/10/data-driven-design-an-integral-part-of-ux-design.php";
    // Group 7: Human Centred Design, System Design
    const group7Diagram = "https://outwitly.com/blog/hcd-process-human-centered-design/";
    // Group 8: ESGs, Greenwashing, Strategy, Product Centred Design, User Centred Design, Methodology, Principles, Empathy, Iterate
    const group8Diagram = "https://maze.co/guides/user-personas/persona-mapping/";

    const termDetails = {
      "Empathize": {
        definition: "<strong>Empathize</strong> is the first phase of HCD where designers immerse themselves in the users’ world.",
        explanation: "This phase involves direct observation, interviews, and immersion in the user's environment to deeply understand their needs, challenges, and aspirations. Insights here form the foundation for user-centered solutions.",
        diagram: "https://media.nngroup.com/media/editor/2017/12/14/screen-shot-2017-12-14-at-55628-pm.png",
        video: "https://www.youtube.com/embed/lsd7kn9NH9U",
        related: ["User Research", "Persona"],
        caseStudies: [
          "Article 1: In a recent project, designers conducted extensive field studies in urban neighborhoods—interviewing over 50 residents. Their insights led to a redesign of the local transit system, significantly improving accessibility.",
          "Article 2: A startup integrated empathy workshops into their process, where designers spent a day with users to uncover subtle challenges. This led to a product that resonated deeply with its audience."
        ],
        quiz: {
          question: "What is the primary goal of the Empathize phase?",
          options: [
            "To understand user needs",
            "To design the final product",
            "To conduct market analysis",
            "To build a prototype"
          ],
          correct: "To understand user needs"
        }
      },
      "Define": {
        definition: "<strong>Define</strong> synthesizes insights from empathy research into a clear problem statement.",
        explanation: "Designers analyze data to identify key issues and patterns, creating a focused brief that guides the design process. This ensures that subsequent ideas address real user problems.",
        diagram: group1Diagram,
        video: "https://www.youtube.com/embed/PYEkso34Ej8",
        related: ["Empathize", "User Research"],
        caseStudies: [
          "Article 1: A design team used research insights to articulate a problem statement that aligned with both user needs and business goals, setting the stage for innovative solutions.",
          "Article 2: A corporation redefined its services by clarifying core user challenges, resulting in targeted and successful innovations."
        ],
        quiz: {
          question: "What is the key outcome of the Define phase?",
          options: [
            "A well-articulated problem statement",
            "A finalized design",
            "A complete set of user requirements",
            "A list of potential solutions"
          ],
          correct: "A well-articulated problem statement"
        }
      },
      "Ideate": {
        definition: "<strong>Ideate</strong> is the brainstorming phase for generating creative solutions.",
        explanation: "Designers employ techniques like brainstorming and mind mapping to explore a broad range of ideas. The focus is on quantity and diversity, ensuring innovative approaches to user challenges.",
        diagram: "https://project-drawify-v2.s3.eu-west-3.amazonaws.com/templates/4/05qwimgl/05qwimgl.jpg",
        video: "https://www.youtube.com/embed/NLcvWV6nsqg",
        related: ["Prototype", "Empathize"],
        caseStudies: [
          "Article 1: During an ideation workshop, a team generated over 200 ideas without judgment. Several ideas evolved into core product features.",
          "Article 2: An agency’s unstructured brainstorming led to groundbreaking concepts that redefined industry standards."
        ],
        quiz: {
          question: "What is the main focus during the Ideate phase?",
          options: [
            "Generating creative ideas",
            "Implementing solutions",
            "Testing prototypes",
            "Analyzing data"
          ],
          correct: "Generating creative ideas"
        }
      },
      "Prototype": {
        definition: "<strong>Prototype</strong> turns ideas into tangible models for testing.",
        explanation: "Prototyping is an iterative process where designs are built in low- to high-fidelity forms. This allows designers to experiment and refine ideas quickly based on user feedback.",
        diagram: "https://cdn.educba.com/academy/wp-content/uploads/2019/05/prototype-model.png",
        video: "https://www.youtube.com/embed/o7lT0z1fLx4",
        related: ["Ideate", "Test"],
        caseStudies: [
          "Article 1: A startup developed several prototypes to test different concepts, learning from each iteration to produce a user-friendly product.",
          "Article 2: In an agile environment, rapid prototyping enabled continuous refinement and user feedback, resulting in a successful design."
        ],
        quiz: {
          question: "What is the primary purpose of prototyping?",
          options: [
            "To create a tangible model for testing",
            "To finalize the product",
            "To analyze market trends",
            "To gather user demographics"
          ],
          correct: "To create a tangible model for testing"
        }
      },
      "Test": {
        definition: "<strong>Test</strong> evaluates prototypes with real users to refine designs.",
        explanation: "User testing gathers feedback and identifies usability issues. This iterative stage is essential for validating that the design meets user needs before final production.",
        diagram: "https://www.d8aspring.com/hubfs/Screen%20Shot%202018-02-15%20at%2016.04.30.png",
        video: "https://www.youtube.com/embed/9oa9WmVTzhU",
        related: ["Prototype", "Define"],
        caseStudies: [
          "Article 1: Through iterative testing sessions, a team refined their product based on real user interactions, significantly improving usability.",
          "Article 2: Extensive user testing uncovered unforeseen issues, allowing the design team to make crucial improvements prior to launch."
        ],
        quiz: {
          question: "What is the purpose of testing in HCD?",
          options: [
            "To gather user feedback and refine designs",
            "To launch a product without changes",
            "To measure market trends",
            "To finalize the design without iteration"
          ],
          correct: "To gather user feedback and refine designs"
        }
      },
      "User Research": {
        definition: "<strong>User Research</strong> involves collecting data to understand user behaviors and needs.",
        explanation: "Using interviews, surveys, and observations, designers gather insights that inform every phase of HCD. These insights are essential for creating solutions that truly resonate with users.",
        diagram: "https://cdn.prod.website-files.com/6515c0b8d248616d9d2aedf6/652ecd48386897205e5d01da_assets_acabc2d7acd7496a84965c121aa866a0_fdfff98c57bc4fb4bc0336d5362172ec.png",
        video: "https://www.youtube.com/embed/GKTiY2bL5p4",
        related: ["Empathize", "Define"],
        caseStudies: [
          "Article 1: A comprehensive study uncovered trends that directly shaped the design strategy, resulting in a user-centric product.",
          "Article 2: Observational research revealed critical pain points, enabling a breakthrough redesign that improved user satisfaction."
        ],
        quiz: {
          question: "What does User Research primarily involve?",
          options: [
            "Gathering data about user behavior and needs",
            "Designing the product interface",
            "Creating promotional materials",
            "Finalizing technical specifications"
          ],
          correct: "Gathering data about user behavior and needs"
        }
      },
      "Journey Mapping": {
        definition: "<strong>Journey Mapping</strong> visually represents the entire user experience over time.",
        explanation: "This technique captures every touchpoint—from first contact to long-term engagement—highlighting both moments of delight and areas for improvement.",
        diagram: "https://media.woopra.com/image/upload/f_auto/v1702077961/customer_journey_maps.png",
        video: "https://www.youtube.com/embed/z8cVwERAndY",
        related: ["Empathize", "Persona", "User Research"],
        caseStudies: [
          "Article 1: A retail brand used journey mapping to uncover service gaps, leading to a redesign that boosted customer loyalty.",
          "Article 2: A service company mapped user emotions to drive transformative changes in their customer support approach."
        ],
        quiz: {
          question: "What is the purpose of Journey Mapping?",
          options: [
            "To visualize the entire customer experience",
            "To design the user interface",
            "To create a list of features",
            "To plan the project timeline"
          ],
          correct: "To visualize the entire customer experience"
        }
      },
      "Persona": {
        definition: "<strong>Persona</strong> is a fictional representation of a user segment.",
        explanation: "Based on research, personas embody the characteristics, goals, and challenges of target user groups, keeping design decisions focused on real user needs.",
        diagram: "https://devsquad-website.s3.us-east-1.amazonaws.com/blog/user-centered-design-process/devsquad-blog-how-a-user-centered-design-process-shapes-product-development-infographic-1_2eae7333981eb486ea6ceb278400f298_800.webp",
        video: "https://www.youtube.com/embed/WtJDgVoJQtw",
        related: ["User Research", "Empathize"],
        caseStudies: [
          "Article 1: Developing detailed personas helped a team tailor their product to diverse user groups, resulting in higher engagement.",
          "Article 2: Vivid personas guided a company in creating relatable and effective marketing strategies."
        ],
        quiz: {
          question: "What is a Persona used for?",
          options: [
            "To represent a segment of real users",
            "To document technical specifications",
            "To define the company mission",
            "To list available features"
          ],
          correct: "To represent a segment of real users"
        }
      },
      "Usability": {
        definition: "<strong>Usability</strong> refers to how easily users can interact with a product.",
        explanation: "Usability testing reveals whether a design is intuitive and accessible. Designers use this feedback to refine interfaces for a smooth user experience.",
        diagram: "https://www.researchgate.net/publication/346567548/figure/fig1/AS:1022608258650114@1620820216517/E-of-the-product-usability.ppm",
        video: "https://www.youtube.com/embed/F-rIZip5WnU",
        related: ["Test", "Prototype"],
        caseStudies: [
          "Article 1: Usability testing uncovered several interface issues, leading to iterative improvements and a more satisfying user experience.",
          "Article 2: A detailed usability analysis resulted in design refinements that significantly reduced user errors."
        ],
        quiz: {
          question: "What does Usability refer to?",
          options: [
            "How effectively a user can interact with a product",
            "The aesthetic appeal of a design",
            "The cost-effectiveness of production",
            "The technical complexity of the product"
          ],
          correct: "How effectively a user can interact with a product"
        }
      },
      "Data Driven Design": {
        definition: "<strong>Data Driven Design</strong> uses user data to inform design decisions.",
        explanation: "This approach combines quantitative metrics and qualitative insights to shape iterations. By analyzing user behavior and feedback, designers create solutions that are both effective and relevant.",
        diagram: "https://bscdesigner.com/wp-content/uploads/2020/07/data-driven-decision-7-steps.png",
        video: "https://www.youtube.com/embed/NBrwt1kFqYg",
        related: ["User Research", "Methodology"],
        caseStudies: [
          "Article 1: A company used extensive analytics to inform design changes, resulting in a more intuitive product interface.",
          "Article 2: Data collected from user interactions led to iterative refinements that boosted engagement and satisfaction."
        ],
        quiz: {
          question: "What is the main focus of Data Driven Design?",
          options: [
            "Using data to inform design",
            "Relying solely on intuition",
            "Ignoring user feedback",
            "Following market trends"
          ],
          correct: "Using data to inform design"
        }
      },
      "Qualitative": {
        definition: "<strong>Qualitative</strong> research captures rich, narrative insights from users.",
        explanation: "In HCD, qualitative methods like interviews and observations provide depth and context. These insights complement quantitative data, revealing the emotions and motivations behind user behavior.",
        diagram: "https://ideascale.com/wp-content/uploads/2023/07/qualitative-research-design-cover.jpg",
        video: "https://www.youtube.com/embed/FCiYajpP4LM",
        related: ["User Research", "Empathize"],
        caseStudies: [
          "Article 1: In-depth interviews uncovered subtle user challenges that numerical data alone couldn’t reveal, driving innovative design solutions.",
          "Article 2: Observational studies provided rich qualitative insights that shaped a more empathetic and user-centric product."
        ],
        quiz: {
          question: "What does qualitative research primarily focus on?",
          options: [
            "Understanding experiences and emotions",
            "Collecting numerical data",
            "Statistical analysis",
            "Market share numbers"
          ],
          correct: "Understanding experiences and emotions"
        }
      },
      "Pivot": {
        definition: "<strong>Pivot</strong> is a strategic shift in design direction based on user feedback.",
        explanation: "When initial concepts do not fully address user needs, a pivot allows designers to rethink assumptions and adjust the approach. This adaptive change is central to the iterative nature of HCD.",
        diagram: "https://miro.medium.com/v2/resize:fit:800/1*XHbsQNEQZUgpqoMxrLO-BQ.png",
        video: "https://www.youtube.com/embed/NBrwt1kFqYg",
        related: ["Ideate", "Iterate"],
        caseStudies: [
          "Article 1: Based on feedback, a design team pivoted from a complex interface to a more streamlined solution, greatly enhancing usability.",
          "Article 2: A startup pivoted their design strategy mid-project, leading to a breakthrough product that better addressed user pain points."
        ],
        quiz: {
          question: "What does pivot mean in HCD?",
          options: [
            "A strategic shift based on feedback",
            "Maintaining the same design",
            "A minor tweak",
            "Ignoring user insights"
          ],
          correct: "A strategic shift based on feedback"
        }
      },
      "Expansion": {
        definition: "<strong>Expansion</strong> broadens the scope of design solutions to include diverse user needs.",
        explanation: "By incorporating feedback from varied sources, designers can scale their solutions to be more inclusive and versatile, ensuring the product works for a wider audience.",
        diagram: "https://public-images.interaction-design.org/tags/iaZacNcchDGVfzW0HRtTmA3akqd49RS32M3a8nfY.png",
        video: "https://www.youtube.com/embed/X0li4FGktmA&list=PLjnP0JesQLr-LDcgb74TzkL7iFbjRvlqh&index=8",
        related: ["Strategy", "User Centred Design"],
        caseStudies: [
          "Article 1: A design team expanded their focus to accommodate a more diverse user base, resulting in a product that appealed to a broader market.",
          "Article 2: Expansion of design parameters led to an inclusive solution that addressed previously unmet needs across various customer segments."
        ],
        quiz: {
          question: "What is the aim of expansion in HCD?",
          options: [
            "To broaden design scope",
            "To narrow focus",
            "To limit user options",
            "To stick with minimal design"
          ],
          correct: "To broaden design scope"
        }
      },
      "Customer Segment": {
        definition: "<strong>Customer Segment</strong> classifies users into groups based on shared needs and behaviors.",
        explanation: "Understanding different customer segments helps tailor design solutions for specific groups. This focus enables personalization and improves the overall user experience.",
        diagram: "https://cdn3.notifyvisitors.com/blog/wp-content/uploads/2020/03/types-of-customer-segment1.jpg",
        video: "https://www.youtube.com/embed/PdQz27oq_uE&list=PLTZYG7bZ1u6qCRHskLgKf6Olzbbuy-5ce&index=2",
        related: ["Persona", "User Research"],
        caseStudies: [
          "Article 1: By identifying distinct customer segments, a team created tailored experiences that resonated with each group.",
          "Article 2: Segmenting customers enabled focused design strategies that increased engagement and conversion rates."
        ],
        quiz: {
          question: "What does Customer Segment refer to?",
          options: [
            "Grouping users with similar needs",
            "Random user grouping",
            "Focusing on one individual",
            "Ignoring user differences"
          ],
          correct: "Grouping users with similar needs"
        }
      },
      "Human Centred Design": {
        definition: "<strong>Human Centred Design</strong> puts the user’s needs, experiences, and emotions at the heart of the design process.",
        explanation: "HCD is an iterative process that emphasizes empathy, prototyping, and testing. Its goal is to create solutions that are not only functional but also meaningful and responsive to real user needs.",
        diagram: "https://public-images.interaction-design.org/tags/iaZacNcchDGVfzW0HRtTmA3akqd49RS32M3a8nfY.png",
        video: "https://www.youtube.com/embed/ezPJ7TVfnl8",
        related: ["Empathy", "User Centred Design"],
        caseStudies: [
          "Article 1: An organization revamped its product by centring every decision around user feedback, resulting in a solution that truly resonated with its audience.",
          "Article 2: Human Centred Design transformed a complex service into an intuitive experience by continuously iterating based on user insights."
        ],
        quiz: {
          question: "What is the core principle of Human Centred Design?",
          options: [
            "Prioritizing user needs",
            "Focusing on aesthetics",
            "Maximizing profit",
            "Following trends"
          ],
          correct: "Prioritizing user needs"
        }
      },
      "System Design": {
        definition: "<strong>System Design</strong> addresses the creation of interconnected and holistic systems.",
        explanation: "In HCD, system design considers how different components interact to create a seamless experience. It ensures that the overall solution is coherent, sustainable, and adaptive to changing user needs.",
        diagram: "https://www.researchgate.net/profile/Remo-Filleti-2/publication/271769283/figure/fig2/AS:302489220993025@1449130458996/Product-system-diagram-example.png",
        video: "https://www.youtube.com/embed/fqNAWyOOVfw&list=PLTZYG7bZ1u6qCRHskLgKf6Olzbbuy-5ce&index=4",
        related: ["Principles", "Strategy"],
        caseStudies: [
          "Article 1: A company restructured its service delivery by redesigning its system architecture, resulting in a more integrated user experience.",
          "Article 2: Holistic system design helped streamline processes and improve overall customer satisfaction in a complex digital platform."
        ],
        quiz: {
          question: "What is a key focus of system design in HCD?",
          options: [
            "Creating interconnected, holistic systems",
            "Designing isolated components",
            "Focusing only on visuals",
            "Ignoring system interactions"
          ],
          correct: "Creating interconnected, holistic systems"
        }
      },
      "ESGs": {
        definition: "<strong>Environment Social Governance (ESGs)</strong> integrates sustainability and ethical considerations into design.",
        explanation: "In HCD, ESGs ensure that design solutions are environmentally responsible, socially beneficial, and ethically governed. This holistic approach promotes long-term sustainability.",
        diagram: "https://cdn.corporatefinanceinstitute.com/assets/esg-environmental-social-governance.jpg",
        video: "https://www.youtube.com/embed/s1VAh51C9Bo&list=PLTZYG7bZ1u6qCRHskLgKf6Olzbbuy-5ce&index=5",
        related: ["Strategy", "Human Centred Design"],
        caseStudies: [
          "Article 1: A design team incorporated ESG principles to create a product that not only served users well but also minimized its environmental impact.",
          "Article 2: Integrating ESGs into the design process led to ethical, sustainable innovations that built long-term customer trust."
        ],
        quiz: {
          question: "What do ESGs promote in HCD?",
          options: [
            "Sustainable and ethical design",
            "Ignoring environmental concerns",
            "Focusing solely on profit",
            "Reducing social impact"
          ],
          correct: "Sustainable and ethical design"
        }
      },
      "Greenwashing": {
        definition: "<strong>Greenwashing</strong> is the misleading portrayal of a product as environmentally friendly.",
        explanation: "Within HCD, avoiding greenwashing means ensuring that claims of sustainability are genuine and backed by real practices. Authenticity and transparency are vital for building user trust.",
        diagram: "https://cdn.shopify.com/s/files/1/0310/0442/6375/files/greenwashing-diagram_2048x2048.jpg?v=1680711568",
        video: "https://www.youtube.com/embed/LSXop-NTfR0",
        related: ["ESGs"],
        caseStudies: [
          "Article 1: A company faced backlash after its claims of sustainability were exposed as greenwashing, prompting a complete redesign of its environmental strategy.",
          "Article 2: Avoiding greenwashing, a brand committed to genuine eco-friendly practices and earned high praise from its user community."
        ],
        quiz: {
          question: "What does greenwashing refer to?",
          options: [
            "Misleading claims about sustainability",
            "Genuine eco-friendly design",
            "Strict environmental regulation",
            "High-quality green design"
          ],
          correct: "Misleading claims about sustainability"
        }
      },
      "Strategy": {
        definition: "<strong>Strategy</strong> is the long-term plan that aligns design with user needs and business goals.",
        explanation: "In HCD, a clear strategy guides every phase of the design process, ensuring that solutions are sustainable and impactful. It involves setting objectives, analyzing the competitive landscape, and iterating based on continuous feedback.",
        diagram: "https://as1.ftcdn.net/jpg/04/17/11/34/1000_F_417113436_pDMv35e0bvFhME1v43w4RBCXFhsGPl44.jpg",
        video: "https://www.youtube.com/embed/ruX-s9VTBSQ",
        related: ["Define", "System Design"],
        caseStudies: [
          "Article 1: A focused design strategy enabled a company to streamline its product development, leading to a successful market launch.",
          "Article 2: Strategic planning in HCD resulted in innovations that balanced user satisfaction with business objectives."
        ],
        quiz: {
          question: "What is the role of strategy in HCD?",
          options: [
            "To guide long-term design efforts",
            "To create random ideas",
            "To ignore market trends",
            "To finalize design without feedback"
          ],
          correct: "To guide long-term design efforts"
        }
      },
      "Product Centred Design": {
        definition: "<strong>Product Centred Design</strong> focuses primarily on product features and functionality.",
        explanation: "While it can yield technically sound products, this approach may overlook the emotional and experiential aspects that HCD values. Balancing product features with user needs is key to creating engaging solutions.",
        diagram: "https://miro.medium.com/v2/resize:fit:1200/1*DAvjKtCrnVBLZMGPQMjWAA.png",
        video: "https://www.youtube.com/embed/fbZJBwxKAXU",
        related: ["User Centred Design"],
        caseStudies: [
          "Article 1: A company that focused solely on product features found that user engagement suffered, prompting a shift toward more holistic design.",
          "Article 2: Product Centred Design produced a robust product but lacked the emotional connection that user feedback later inspired."
        ],
        quiz: {
          question: "What distinguishes Product Centred Design from HCD?",
          options: [
            "Focus on features over user experience",
            "Prioritizing user emotions",
            "Emphasizing iterative testing",
            "Incorporating empathy"
          ],
          correct: "Focus on features over user experience"
        }
      },
      "User Centred Design": {
        definition: "<strong>User Centred Design</strong> places users at the core of the design process.",
        explanation: "UCD involves iterative research, prototyping, and testing to ensure that products are tailored to user needs. It is a fundamental pillar of HCD that ensures designs are intuitive, accessible, and engaging.",
        diagram: "https://www.researchgate.net/profile/Atiyeh-Vaezipour/publication/316680273/figure/fig3/AS:669591177019403@1536654385320/The-User-Centred-Design-Approach-adapted-from-ISO-2010.jpg",
        video: "https://www.youtube.com/embed/ruX-s9VTBSQ",
        related: ["Persona", "Empathize"],
        caseStudies: [
          "Article 1: By consistently involving users in testing, a team developed a product that aligned perfectly with their expectations.",
          "Article 2: User Centred Design led to a highly intuitive interface that improved customer retention and satisfaction."
        ],
        quiz: {
          question: "What is the primary focus of User Centred Design?",
          options: [
            "Designing with the user in mind",
            "Focusing on technical specifications",
            "Prioritizing cost reduction",
            "Emphasizing aesthetic design"
          ],
          correct: "Designing with the user in mind"
        }
      },
      "Methodology": {
        definition: "<strong>Methodology</strong> is the structured approach used throughout the HCD process.",
        explanation: "This includes frameworks, techniques, and tools that guide designers in understanding users, generating ideas, prototyping, and testing. A robust methodology ensures consistency and quality in design outcomes.",
        diagram: "https://www.researchgate.net/profile/Sunil-Chahal/publication/373981863/figure/fig2/AS:11431281189194657@1694953233723/Framework-of-Agile-Product-Management.png",
        video: "https://www.youtube.com/embed/NBrwt1kFqYg",
        related: ["Strategy", "Principles"],
        caseStudies: [
          "Article 1: A clearly defined methodology enabled a team to streamline their process and consistently deliver user-focused solutions.",
          "Article 2: By adhering to a structured methodology, a design group achieved rapid iterations and measurable improvements in user satisfaction."
        ],
        quiz: {
          question: "What does methodology provide in HCD?",
          options: [
            "A structured approach to design",
            "Random brainstorming",
            "Final design specifications",
            "Marketing strategies"
          ],
          correct: "A structured approach to design"
        }
      },
      "Principles": {
        definition: "<strong>Principles</strong> are the core values that guide the HCD process.",
        explanation: "These include empathy, iteration, inclusivity, and sustainability. Principles serve as a compass for design decisions, ensuring that solutions remain ethical and user-focused.",
        diagram: "https://miro.medium.com/v2/resize:fit:1400/format:webp/1*3cuMXu3Eo-ALJlAfZ1KsCA.png",
        video: "https://www.youtube.com/embed/fZG22kqCMRU",
        related: ["Human Centred Design", "Methodology"],
        caseStudies: [
          "Article 1: Adhering to HCD principles helped a team create a product that was not only functional but also deeply engaging and ethical.",
          "Article 2: Guiding design decisions with clear principles led to consistent, high-quality outcomes that resonated with users."
        ],
        quiz: {
          question: "What do HCD principles guide?",
          options: [
            "User-focused design decisions",
            "Strict technical development",
            "Profit maximization",
            "Trend following"
          ],
          correct: "User-focused design decisions"
        }
      },
      "Empathy": {
        definition: "<strong>Empathy</strong> is the ability to understand and share the feelings of users.",
        explanation: "In HCD, empathy drives the entire process. By genuinely connecting with users, designers gain critical insights that lead to compassionate and effective solutions.",
        diagram: "https://media.nngroup.com/media/editor/2017/12/14/screen-shot-2017-12-14-at-55628-pm.png",
        video: "https://www.youtube.com/embed/FCiYajpP4LM",
        related: ["Human Centred Design", "User Research"],
        caseStudies: [
          "Article 1: Deep empathy allowed a design team to uncover unspoken user needs, resulting in a product that truly resonated with its audience.",
          "Article 2: Empathetic research led to innovative design choices that enhanced the overall user experience and trust."
        ],
        quiz: {
          question: "What is the role of empathy in HCD?",
          options: [
            "To understand user emotions",
            "To define technical requirements",
            "To improve production speed",
            "To focus on aesthetics"
          ],
          correct: "To understand user emotions"
        }
      },
      "Iterate": {
        definition: "<strong>Iterate</strong> means to continuously refine and improve design solutions.",
        explanation: "Iteration is a core aspect of HCD, allowing designers to learn from each version of a prototype. Through constant refinement based on user feedback, the final product becomes more aligned with user needs.",
        diagram: "https://project-drawify-v2.s3.eu-west-3.amazonaws.com/templates/4/05qwimgl/05qwimgl.jpg",
        video: "https://www.youtube.com/embed/FCiYajpP4LM&utm_source=chatgpt.com",
        related: ["Prototype", "Test"],
        caseStudies: [
          "Article 1: Iterative improvements based on user testing led to a design that was both intuitive and engaging.",
          "Article 2: A series of iterations transformed a rough concept into a polished product that exceeded user expectations."
        ],
        quiz: {
          question: "What does it mean to iterate in HCD?",
          options: [
            "To refine design solutions continuously",
            "To finalize the design immediately",
            "To ignore user feedback",
            "To stop at the first version"
          ],
          correct: "To refine design solutions continuously"
        }
      }
    };

    function loadTerm(termId) {
      // Hide the modal (if open)
      modal.style("display", "none");
      // Get term details; if not available, use fallback content
      const details = termDetails[termId] || {
        definition: "<strong>" + termId + "</strong> definition not available.",
        explanation: "Detailed explanation not available.",
        diagram: "https://via.placeholder.com/750x300?text=" + encodeURIComponent(termId + " Diagram"),
        video: "https://www.youtube.com/embed/dQw4w9WgXcQ",
        related: [],
        caseStudies: [],
        quiz: {
          question: "No quiz available.",
          options: [],
          correct: ""
        }
      };
      document.getElementById("termTitle").textContent = termId;
      document.getElementById("termDefinition").innerHTML = details.definition;
      document.getElementById("termExplanation").textContent = details.explanation;
      document.getElementById("termDiagram").src = details.diagram;
      document.getElementById("termVideo").src = details.video;
      // Populate related terms list
      const relatedList = document.getElementById("relatedTerms");
      relatedList.innerHTML = "";
      details.related.forEach(rel => {
        const li = document.createElement("li");
        const a = document.createElement("a");
        a.href = "#";
        a.textContent = rel;
        a.onclick = () => { loadTerm(rel); return false; };
        li.appendChild(a);
        relatedList.appendChild(li);
      });
      // Populate case studies list with full-text articles
      const caseStudiesList = document.getElementById("caseStudies");
      caseStudiesList.innerHTML = "";
      details.caseStudies.forEach(cs => {
        const li = document.createElement("li");
        li.innerHTML = "<strong>Article:</strong> " + cs;
        caseStudiesList.appendChild(li);
      });
      // Populate quiz dynamically
      const quizForm = document.getElementById("quizForm");
      quizForm.innerHTML = "";
      if(details.quiz && details.quiz.options.length > 0) {
        let quizHtml = `<p>${details.quiz.question}</p>`;
        details.quiz.options.forEach(option => {
          quizHtml += `<label><input type="radio" name="question1" value="${option}"> ${option}</label><br>`;
        });
        quizHtml += `<button type="button" onclick="checkQuiz('${details.quiz.correct}')">Submit Answer</button>`;
        quizForm.innerHTML = quizHtml;
      } else {
        quizForm.innerHTML = "<p>No quiz available for this term.</p>";
      }
      // Reset quiz feedback and comments
      document.getElementById("quizFeedback").textContent = "";
      document.getElementById("commentsList").innerHTML = "";
      document.getElementById("commentBox").value = "";
      // Switch view: hide network view and show term detail view
      document.getElementById("networkView").classList.remove("active");
      document.getElementById("termView").classList.add("active");
    }

    function goBack() {
      // Switch back to the network view
      document.getElementById("termView").classList.remove("active");
      document.getElementById("networkView").classList.add("active");
    }

    /* ---------------------- Term Quiz Functionality ---------------------- */
    function checkQuiz(correctAnswer) {
      const form = document.getElementById("quizForm");
      const feedback = document.getElementById("quizFeedback");
      const selected = form.question1.value;
      if(selected === correctAnswer){
        feedback.textContent = "Correct! " + correctAnswer + " is the right answer.";
      } else {
        feedback.textContent = "Incorrect. Please try again.";
      }
    }

    /* ---------------------- Comments Functionality ---------------------- */
    function submitComment() {
      const commentBox = document.getElementById("commentBox");
      const commentText = commentBox.value.trim();
      if(commentText !== ""){
        const commentsList = document.getElementById("commentsList");
        const li = document.createElement("li");
        li.textContent = commentText;
        commentsList.appendChild(li);
        commentBox.value = "";
      }
    }

    /* ---------------------- Accessibility Functions ---------------------- */
    let currentFontSize = 16;
    function changeFontSize(action) {
      if(action === "increase"){
        currentFontSize += 2;
      } else if(action === "decrease" && currentFontSize > 10){
        currentFontSize -= 2;
      }
      document.body.style.fontSize = currentFontSize + "px";
    }
    function toggleContrast() {
      const body = document.body;
      if(body.style.backgroundColor === "black"){
        body.style.backgroundColor = "#e0e0e0";
        body.style.color = "#333";
      } else {
        body.style.backgroundColor = "black";
        body.style.color = "white";
      }
    }
  </script>
</body>
</html>
