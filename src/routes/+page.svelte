<script lang="ts">
  import Terminal from "$lib/Terminal.svelte";
  const name: string = "Nitesh Patel";
</script>

<section class="hero">
  <h1>{name}</h1>
  <p class="subtitle">Software Engineer & Competitive Programmer</p>
</section>

<section class="about">
  <p>
    Hello, I'm Nitesh Patel. I am an enthusiastic software engineer with a strong background in competitive programming and system development.
  </p>
  <p>
    I enjoy building practical applications and participating in coding challenges to continuously improve my problem-solving skills.
  </p>
  <p>
    My technical stack includes <strong>C++, Kotlin, Svelte, SQLite, and PostgreSQL</strong>. Over the years, I have actively participated in competitive programming, having solved over 200 problems on Codeforces and numerous easy-level problems on LeetCode to build a solid foundation in data structures and algorithms.
  </p>
</section>

<Terminal />

<section class="navigation">
  <h2>Explore</h2>
  <ul class="nav-links">
    <li><a href="/projects">Projects</a></li>
    <li><a href="/posts">Posts</a></li>
  </ul>
</section>

<section class="projects">
  <h2>Recent Projects</h2>
  <ul class="project-list">
    <li><a href="/projects#tree_tracker">Tree Tracker</a> - Application for tracking trees</li>
    <li><a href="/projects#leach_py">LEACH_PY</a> - Python implementation of LEACH protocol</li>
    <li><a href="/projects#pocketlog">PocketLog</a> - Logging application</li>
  </ul>
</section>

<section class="contact">
  <h2>Contact</h2>
  <p>Feel free to get in touch for collaborations, questions, or just to say hi!</p>
  <ul class="contact-links">
    <li>Email: <a href="mailto:your@email.com">your@email.com</a></li>
    <li>GitHub: <a href="https://github.com/yourusername">@yourusername</a></li>
    <li>X/Twitter: <a href="https://x.com/yourhandle">@yourhandle</a></li>
    <li>LinkedIn: <a href="https://linkedin.com/in/yourprofile">Your Name</a></li>
  </ul>
</section>

<style>
  section {
    margin-bottom: 3.5rem;
  }
  .hero h1 {
    margin-bottom: 0.2rem;
  }
  .subtitle {
    color: var(--text-muted);
    font-size: 1.1rem;
    margin-top: 0;
  }
  ul {
    list-style: none;
    padding: 0;
  }
  ul li {
    margin-bottom: 0.8rem;
  }
  .nav-links, .contact-links, .project-list {
    display: flex;
    flex-direction: column;
    gap: 0.2rem;
  }
</style>
