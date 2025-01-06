<div class="container">
  <h1>Welcome to SvelteKit</h1>
  <p>Visit <a href="https://kit.svelte.dev">kit.svelte.dev</a> to read the documentation</p>

  <h1>
      Hello world!
  </h1>
  <div class="test">
    <p>
      Test container
    </p>
  </div>
</div>

  
<style>

    .container {
      padding: 2rem 0;
      width: 85%;
      margin: 0 auto;
      display: block;
      background-image: linear-gradient(
          to right,
          hsl(0, 0%, 40%) calc(50% - max(1.5rem, 3vw)),    /* Left border */
          transparent calc(50% - max(1.5rem, 3vw)),         /* Start gap */
          transparent calc(50% + max(1.5rem, 3vw)),         /* End gap */
          hsl(0, 0%, 40%) calc(50% + max(1.5rem, 3vw))     /* Right border */
      );
      background-size: 100% 1px;
      background-position: bottom;
      background-repeat: no-repeat;
    }

    .test {
      width: 100%;
      height: 1000px;
      border: 2px solid #fff;
      background-image: repeating-linear-gradient(
          45deg,
          transparent,
          transparent 10px,
          hsla(0, 0%, 100%, 0.5) 10px,
          hsla(0, 0%, 100%, 0.5) 20px
      );
      display: flex;
      justify-content: center;
      align-items: center;
    }

    .test p {
      color: #fff;
      font-size: 2rem;
      padding: 2rem;
      background-color: hsla(0, 0%, 16%);
    }

    .container > * {
      margin-bottom: 1rem;
    }

</style>