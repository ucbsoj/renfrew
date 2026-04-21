<script lang="ts">
  import rawBlocks from '$lib/doc.blocks.json';
  import DocRenderer from '$lib/components/DocRenderer.svelte';
  import type { Block } from '$lib/components/DocRenderer.svelte';
  const blocks = rawBlocks as Block[];

  import { onMount } from "svelte";

  let scroller: HTMLElement | null = null;
  let bgVideo: HTMLVideoElement | null = null;

  let blur = 30;
  let canPlay = false;


  function handleScroll() {
    if (!scroller || !bgVideo) return;

    const rect = scroller.getBoundingClientRect();
    const vh = window.innerHeight;

    let progress = (vh - rect.top) / (rect.height + vh);

    if (progress < 0.55) {
      blur = 30;
      canPlay = false;
      bgVideo.pause();
    } else {
      canPlay = false;
      bgVideo.pause();
      const start = 0.55;
      const end = 0.70; // 👈 finish earlier

      const t = (progress - start) / (end - start);
      blur = Math.max(30 - t * 30, 0);

      if (blur === 0) {
        canPlay = true;
      }
    }
    
  }

  onMount(() => {
    window.addEventListener("scroll", handleScroll, { passive: true });
    handleScroll();

    return () => window.removeEventListener("scroll", handleScroll);
  });
</script>



<!-- 
Hard-code custom code that should appear BEFORE Google Doc here. 
This code is in +page.svelte and can be manually coded using HTML or Svelte tags.
-->
<!-- <header class="container-fluid d-flex flex-column align-items-center justify-content-center vh-100 mb-4" style="background-image:url('hero-example.png');background-size:cover;background-position:center;">
  <h1 class="text-white text-center display-1">Multimedia Template 2026</h1>
  <p class="text-white fw-light text-center">This is an example of the 2026 <a class="text-light" href="https://github.com/jrue/multimedia-template-2026" target="_blank">multimedia template</a> using Svelte.<br> This topper (header) is hard coded in <code>+page.svelte</code> file, while the rest of the page is imported from a <a class="text-light" href="https://docs.google.com/document/d/1JaxRkCqZyeWIPwXXocAwUoFaPsqULiQI_Oiby_F0JZ8/edit?usp=sharing" target="_blank">Google Doc</a>.</p>
</header> -->


<!-- DO NOT EDIT (except to modify classes). This will become code imported from Google Doc. -->
<!-- <div class="container">
  <div class="row justify-content-center">
    <div class="col-12 col-sm-10 col-lg-8 col-xxl-6">
      <DocRenderer {blocks} />
    </div>
  </div>
</div> -->

<div class="headScroller" bind:this={scroller}>
   <div 
    class="bg"
    class:active={canPlay}
    style="--blur: {blur}px"
  >
    <div class="videoWrap">
      <video
        bind:this={bgVideo}
        muted
        playsinline
        preload="metadata"
        controls={canPlay}   
      >
        <source src="https://interactive-examples.mdn.mozilla.net/media/cc0-videos/flower.mp4" type="video/mp4" />
      </video>
    </div>
  </div>
  <div class="sliderContent">
    <div class="slide">
      <h1>This business killed my son: A California hospital empire rises as patients suffer</h1>
    </div>
    <div class="slide">
      <p>California is sending more people in mental health crises to locked facilities run by for-profit companies. The largest operator is Signature Healthcare Services.</p>
    </div>
    <div class="slide">
      <p>Signature uses controversial strategies that shift money out of the hospitals and strain their finances. In one major deal, the company's owner sold the real estate at six facilities, including Vista del Mar Hospital in Ventura, for $380 million.</p>
    </div>
    <div class="slide">
      <p>“Any patient death is a tragedy.”</p>
    </div>
     <div class="slide"></div>
  </div>
</div>

<div class="mainContainer">
  <DocRenderer {blocks} />
</div>


<!-- Hard-code any custom code that should appear AFTER Google Doc below here. -->
<footer class="container-fluid bg-dark text-white p-5">
  <p class="text-center">See template on <a class="text-white" href="https://github.com/jrue/multimedia-template-2026" target="_blank">Github</a></p>
</footer>