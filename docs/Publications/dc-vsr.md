---
layout: default
title: DC-VSR: Spatially and Temporally Consistent Video Super-Resolution with Video Diffusion Prior
parent: Publications
---
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <meta name="description" content="DC-VSR: Spatially and Temporally Consistent Video Super-Resolution with Video Diffusion Prior">
  <meta name="keywords" content="Video Super-Resolution, Video Diffusion Prior, Diffusion Model">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>DC-VSR for video super-resolution</title>

  <link rel="icon" href="./assets/images/favicon.ico" type="image/x-icon">

  <style>
    .hr {width: 100%; height: 1px; margin: 48px 0; background-color: #d6dbdf;}
    .beautiful-text {
        font-size: 48px;
        font-weight: bold;
        text-align: center;
        background: linear-gradient(45deg, #ff4747, #ffa845, #ffd900, #BAFFC9, #76c2fc, #ae3cff); /* 파스텔 톤 그라데이션 */
        background-size: 400% 400%;
        -webkit-background-clip: text;
        -webkit-text-fill-color: transparent;
        animation: gradientAnimation 10s ease infinite;
    }

    /* 그라데이션 애니메이션 */
    @keyframes gradientAnimation {
        0% {
            background-position: 0% 50%;
        }
        50% {
            background-position: 100% 50%;
        }
        100% {
            background-position: 0% 50%;
        }
    }
  </style>


  <link href="https://fonts.googleapis.com/css?family=Google+Sans|Noto+Sans|Castoro" rel="stylesheet">
  <link rel="stylesheet" href="./assets/css/bulma.min.css">
  <link rel="stylesheet" href="./assets/css/bulma-carousel.min.css">
  <link rel="stylesheet" href="./assets/css/bulma-slider.min.css">
  <link rel="stylesheet" href="./assets/css/fontawesome.all.min.css">
  <link rel="stylesheet" href="./assets/css/index.css">
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/gh/jpswalsh/academicons@1/css/academicons.min.css">

  <!-- custom css file  -->
  <link rel="stylesheet" href="./assets/css/style.css">
  <link rel="stylesheet" href="./assets/css/twentytwenty.css">
  <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
</head>
<body>

  <section class="hero">
    <div class="hero-body">
      <div class="container is-max-desktop head">
        <div class="columns is-centered">
          <div class="column has-text-centered head">
            <h1 class="beautiful-text main_title is-2 publication-title"> 
              <span>DC-VSR</span>
            </h1>
            <h1 class="title is-3 publication-title"> 
              Spatially and Temporally Consistent Video Super-Resolution with Video Diffusion Prior
            </h1>
  
            <div class="is-size-5 publication-authors">
              <span class="author-block">
                <a href="https://cg.postech.ac.kr/">Janghyeok Han<sup>1&#8224</sup></a>&nbsp;&nbsp;</span>
              <span class="author-block">
                <a href="https://cg.postech.ac.kr/">Gyujin Sim<sup>1&#8224</sup></a>&nbsp;&nbsp;</span>
              <span class="author-block">
                <a href="https://kimgeonung.github.io/">Geonung Kim<sup>1</sup></a>&nbsp;&nbsp;</span> 
              <span class="author-block">
                <a href="https://cg.postech.ac.kr/">Hyun-Seung Lee<sup>2</sup></a>&nbsp;&nbsp;</span> 
              <span class="author-block">
                <a href="https://cg.postech.ac.kr/">Kyuha Choi<sup>2</sup></a>&nbsp;&nbsp;</span> 
              <span class="author-block">
                <a href="https://cg.postech.ac.kr/">Youngseok Han<sup>2</sup></a>&nbsp;&nbsp;</span> 
              <span class="author-block">
                <a href="https://www.scho.pe.kr/">Sunghyun Cho<sup>1</sup></a></span> 
            </div>

            <div class="is-size-5 publication-authors">
              <span class="author-block"><sup>1</sup>POSTECH,&nbsp;&nbsp;<sup>2</sup>Visual Display Business, Samsung Electronics</span>
            </div>

          <!-- <div class="is-size-5 publication-authors">
            <span class="author-block">ICCV 2023</span>
          </div> -->

          <div class="is-size-6 publication-authors">
            <span class="author-block"><sup>&#8224</sup>Equal contribution</span>
          </div>

          <div class="column has-text-centered">
            <div class="publication-links">
              <!-- PDF Link. -->
              <!-- <span class="link-block">
                <a href="./xx.pdf" target="_blank"
                   class="external-link button is-normal is-rounded is-dark">
                  <span class="icon">
                      <i class="fas fa-file-pdf"></i>
                  </span>
                  <span>Paper</span>
                </a>
              </span> -->
              <!-- arXiv Link. -->
              <span class="link-block">
                <a href="https://arxiv.org/abs/2502.03502" target="_blank"
                   class="external-link button is-normal is-rounded is-dark">
                  <span class="icon">
                      <i class="ai ai-arxiv"></i>
                  </span>
                  <span>arXiv</span>
                </a>
              </span>

              <!-- Code Link. -->
              <span class="link-block"> 
                <a href="https://github.com/DaramGC/DC-VSR/tree/main" target="_blank"
                   class="external-link button is-normal is-rounded is-dark">
                  <span class="icon">
                      <i class="fab fa-github"></i>
                  </span>
                  <span>Code</span>
                  </a>
              </span>

              <!-- Video Link. -->
              <span class="link-block">
                <a href="https://www.youtube.com/watch?v=EaHrA9bcMSs" target="_blank"
                   class="external-link button is-normal is-rounded is-dark">
                  <span class="icon">
                      <i class="fab fa-youtube"></i>
                  </span>
                  <span>Video</span>
                </a>
              </span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</section>

<!-- teaser -->
<section class="hero is-light is-small">
  <div class="hero-body">
    <div class="container">
      <div id="results-carousel" class="carousel results-carousel" data-slides-to-show="1" data-loop="true">

        <div class="item">
          <div class="twentytwenty-container" data-orientation="horizontal" ratio="0.5625">
            <div class="desc">Classic Animation Video</div>
            <div class="video">
              <video muted autoplay="autoplay" loop="loop" width="100%">
                <source src="./assets/videos/_anime_00.mp4" type="video/mp4">
              </video> 
            </div>
            <div class="video">
              <video muted autoplay="autoplay" loop="loop" width="100%">
                <source src="./assets/videos/_sr_anime_00.mp4" type="video/mp4">
              </video>
            </div>
          </div>
        </div>

        <div class="item">
          <div class="twentytwenty-container" data-orientation="horizontal" ratio="0.5625">
            <div class="desc">AIGC Video</div>
            <div class="video">
              <video muted autoplay="autoplay" loop="loop" width="100%">
                <source src="./assets/videos/_12_speedup30fps.mp4" type="video/mp4">
              </video> 
            </div>
            <div class="video">
              <video muted autoplay="autoplay" loop="loop" width="100%">
                <source src="./assets/videos/_sr_12.mp4" type="video/mp4">
              </video> 
            </div>
          </div>
        </div>

        <div class="item">
          <div class="twentytwenty-container" data-orientation="horizontal" ratio="0.5625">
            <div class="desc">Old Movie</div>
            <div class="video">
              <video muted autoplay="autoplay" loop="loop" width="100%">
                <source src="./assets/videos/_002.mp4" type="video/mp4">
              </video> 
            </div>
            <div class="video">
              <video muted autoplay="autoplay" loop="loop" width="100%">
                <source src="./assets/videos/_sr002.mp4" type="video/mp4">
              </video>
            </div>
          </div>
        </div>

      </div>
  </div>
</section>

<!-- first row -->
<section class="hero is-light is-small">
  <div class="hero-body">
    <div class="container">
      <div id="results-carousel" class="carousel results-carousel">

        <div class="item">
          <div class="twentytwenty-container" data-orientation="horizontal" ratio="0.5625">
            <div class="desc">Classic Animation Video</div>
            <div class="video">
              <video muted autoplay="autoplay" loop="loop" width="100%">
                <source src="./assets/videos/_006.mp4" type="video/mp4">
              </video> 
            </div>
            <div class="video">
              <video muted autoplay="autoplay" loop="loop" width="100%">
                <source src="./assets/videos/_sr006.mp4" type="video/mp4">
              </video>
            </div>
          </div>
        </div>

        <div class="item">
          <div class="twentytwenty-container" data-orientation="horizontal" ratio="0.5625">
            <div class="desc">AIGC Video</div>
            <div class="video">
              <video muted autoplay="autoplay" loop="loop" width="100%">
                <source src="./assets/videos/_00_speedup30fps.mp4" type="video/mp4">
              </video> 
            </div>
            <div class="video">
              <video muted autoplay="autoplay" loop="loop" width="100%">
                <source src="./assets/videos/_sr_00.mp4" type="video/mp4">
              </video> 
            </div>
          </div>
        </div>

        <div class="item">
          <div class="twentytwenty-container" data-orientation="horizontal" ratio="0.5625">
            <div class="desc">AIGC Video</div>
            <div class="video">
              <video muted autoplay="autoplay" loop="loop" width="100%">
                <source src="./assets/videos/_13_speedup30fps.mp4" type="video/mp4">
              </video> 
            </div>
            <div class="video">
              <video muted autoplay="autoplay" loop="loop" width="100%">
                <source src="./assets/videos/_sr_13.mp4" type="video/mp4">
              </video>
            </div>
          </div>
        </div>

        <div class="item">
          <div class="twentytwenty-container" data-orientation="horizontal" ratio="0.5625">
            <div class="desc">AIGC Video</div>
            <div class="video">
              <video muted autoplay="autoplay" loop="loop" width="100%">
                <source src="./assets/videos/_14_speedup30fps.mp4" type="video/mp4">
              </video> 
            </div>
            <div class="video">
              <video muted autoplay="autoplay" loop="loop" width="100%">
                <source src="./assets/videos/_sr_14.mp4" type="video/mp4">
              </video>
            </div>
          </div>
        </div>

      </div>
  </div>
</section>

<!-- second row -->
<section class="hero is-light is-small">
  <div class="hero-body">
    <div class="container">
      <div id="results-carousel" class="carousel results-carousel">

        <div class="item">
          <div class="twentytwenty-container" data-orientation="horizontal" ratio="0.5625">
            <div class="desc">VideoLQ</div>
            <div class="video">
              <video muted autoplay="autoplay" loop="loop" width="100%">
                <source src="./assets/videos/_012.mp4" type="video/mp4">
              </video> 
            </div>
            <div class="video">
              <video muted autoplay="autoplay" loop="loop" width="100%">
                <source src="./assets/videos/_sr012.mp4" type="video/mp4">
              </video> 
            </div>
          </div>
        </div>

        <div class="item">
          <div class="twentytwenty-container" data-orientation="horizontal" ratio="0.5625">
            <div class="desc">VideoLQ</div>
            <div class="video">
              <video muted autoplay="autoplay" loop="loop" width="100%">
                <source src="./assets/videos/_014.mp4" type="video/mp4">
              </video> 
            </div>
            <div class="video">
              <video muted autoplay="autoplay" loop="loop" width="100%">
                <source src="./assets/videos/_sr014.mp4" type="video/mp4">
              </video> 
            </div>
          </div>
        </div>

        <div class="item">
          <div class="twentytwenty-container" data-orientation="horizontal" ratio="0.5625">
            <div class="desc">VideoLQ</div>
            <div class="video">
              <video muted autoplay="autoplay" loop="loop" width="100%">
                <source src="./assets/videos/_024.mp4" type="video/mp4">
              </video> 
            </div>
            <div class="video">
              <video muted autoplay="autoplay" loop="loop" width="100%">
                <source src="./assets/videos/_sr024.mp4" type="video/mp4">
              </video> 
            </div>
          </div>
        </div>

        <div class="item">
          <div class="twentytwenty-container" data-orientation="horizontal" ratio="0.5625">
            <div class="desc">VideoLQ</div>
            <div class="video">
              <video muted autoplay="autoplay" loop="loop" width="100%">
                <source src="./assets/videos/_026.mp4" type="video/mp4">
              </video> 
            </div>
            <div class="video">
              <video muted autoplay="autoplay" loop="loop" width="100%">
                <source src="./assets/videos/_sr026.mp4" type="video/mp4">
              </video> 
            </div>
          </div>
        </div>

        <div class="item">
          <div class="twentytwenty-container" data-orientation="horizontal" ratio="0.5625">
            <div class="desc">VideoLQ</div>
            <div class="video">
              <video muted autoplay="autoplay" loop="loop" width="100%">
                <source src="./assets/videos/_033.mp4" type="video/mp4">
              </video> 
            </div>
            <div class="video">
              <video muted autoplay="autoplay" loop="loop" width="100%">
                <source src="./assets/videos/_sr033.mp4" type="video/mp4">
              </video> 
            </div>
          </div>
        </div>

        <div class="item">
          <div class="twentytwenty-container" data-orientation="horizontal" ratio="0.5625">
            <div class="desc">VideoLQ</div>
            <div class="video">
              <video muted autoplay="autoplay" loop="loop" width="100%">
                <source src="./assets/videos/_036.mp4" type="video/mp4">
              </video> 
            </div>
            <div class="video">
              <video muted autoplay="autoplay" loop="loop" width="100%">
                <source src="./assets/videos/_sr036.mp4" type="video/mp4">
              </video> 
            </div>
          </div>
        </div>

      </div>
  </div>
</section>



<section class="teaser">
  <div class="container is-max-desktop">
      <div class="columns is-centered has-text-centered">
        <!-- <div class="column is-four-fifths"> -->
        <div class="column">
          <div class="content main_title">
            <br> <b>DC-VSR</b> is video super-resolution model that breathes life into low-res footage with fine detail and clarity.
          </div>
          <div class="hr"></div>
        </div>
      </div>
  </div>
</section>

<section class="section">
  <div class="container is-max-desktop">
    <!-- Abstract. -->
    <div class="columns is-centered has-text-centered">
      <div class="column is-four-fifths">
      <!-- <div class="column"> -->
        <h2 class="title is-3">Abstract</h2>
        <div class="content has-text-justified">
          <p>
            Video super-resolution (VSR) aims to reconstruct a high-resolution (HR) video from a low-resolution (LR) counterpart. 
            Achieving successful VSR requires producing realistic HR details and ensuring both spatial and temporal consistency. 
            To restore realistic details, diffusion-based VSR approaches have recently been proposed. 
            However, the inherent randomness of diffusion, combined with their tile-based approach, often leads to spatio-temporal inconsistencies. 
            In this paper, we propose <b>DC-VSR</b>, a novel VSR approach to produce spatially and temporally consistent VSR results with realistic textures. 
            To achieve spatial and temporal consistency, <b>DC-VSR</b> adopts a novel <b>Spatial Attention Propagation (SAP)</b> scheme and a <b>Temporal Attention Propagation (TAP)</b> scheme that propagate information across spatio-temporal tiles based on the self-attention mechanism. 
            To enhance high-frequency details, we also introduce <b>Detail-Suppression Self-Attention Guidance (DSSAG)</b>, a novel diffusion guidance scheme. 
            Comprehensive experiments demonstrate that DC-VSR achieves spatially and temporally consistent, high-quality VSR results, outperforming previous approaches.
          </p>
        </div>
      </div>
    </div>
    <!--/ Abstract. -->
  </div>
</section>

<section class="section">
  <div class="container is-max-desktop">
    <div class="columns is-centered has-text-centered">
      <!-- <div class="column is-four-fifths"> -->
      <div class="column">
        <h2 class="title is-3">Method</h2>
        <div class="content has-text-justified">
          <p>
            DC-VSR enhances low-resolution videos to any high-resolution videos.
            To deal with any size of videos, DC-VSR processes inputs with overlapping spatio-temporal tiles.
            However, tile-based approach has inconsistency problem among separated tiles.
            By employing Spatial Attention Propagation (SAP) and Temporal Attention Propagation (TAP), 
            the sharing of information among spatio-temporal tiles leads to spatially and temporally consistent high-resolution videos.
            Furthermore, to improve the video frame quality, latents in each diffusion timesteps go through Detail-Suppression Self-Attention Guidance (DSSAG).
            DSSAG leads model to generate fine detail and clear frames.
          </p>
          <div class="column">
            <img src="./assets/images/pipeline.png" />
          </div>
        </div>
      </div>
    </div>
  </div>
</section>

<!-- Paper video. -->
<div id="spotlight-video">
  <div class="container is-max-desktop">
      <div class="column">
        <h2 class="title is-3">
          <span class="a">DC-VSR</span>
          <span class="b">Video</span>
        </h2>
        <div class="publication-video">
          <iframe 
            src="https://www.youtube.com/embed/EaHrA9bcMSs?rel=0&showinfo=0" 
            frameborder="0" 
            allow="autoplay; encrypted-media" 
            allowfullscreen>
          </iframe>
        </div>
    </div>
  </div>
</div>

<section class="section" id="BibTeX">
  <div class="container is-max-desktop content">
    <h2 class="title">BibTeX</h2>
    <pre><code>@InProceedings{janghyeok2025dcvsr,
      title     = {{DC-VSR}: Spatially and Temporally Consistent Video Super-Resolution with Video Diffusion Prior},
      author    = {Janghyeok, Han and Gyujin, Sim and Geonung, Kim and Hyun-Seung, Lee and Kyuha, Choi and Youngseok, Han and Sunghyun, Cho},
      journal   = {arXiv preprint arXiv:2502.03502},
      year      = {2025},
    }</code></pre>
  </div>
</section>


<footer class="footer">
  <div class="container">
    <div class="content has-text-centered">
      <a class="icon-link" href="https://arxiv.org/abs/2502.03502" target="_blank">
        <i class="fas fa-file-pdf"></i>
      </a>
      <a class="icon-link" href="https://github.com/DaramGC/DC-VSR/tree/main" target="_blank">
        <i class="fab fa-github"></i>
      </a>
      <a class="icon-link" href="https://www.youtube.com/watch?v=EaHrA9bcMSs" target="_blank">
        <i class="fab fa-youtube"></i>
      </a>
    </div>

    <div class="columns is-centered">
      <div class="column is-8">
        <div class="content">
          <div class="columns is-centered"><p>
            Thanks to <a href="https://github.com/nerfies/nerfies.github.io" target="_blank">Nerfies</a> and <a href="https://shangchenzhou.com/projects/upscale-a-video/" target="_blank">Upscale-A-Video</a> for the website template. Thank you very much!
          </p></div>
        </div>
      </div>
    </div>

  </div>
</footer>

  <!-- custom js file  -->
  <!-- <script defer src="./assets/js/fontawesome.all.min.js"></script> -->
  <script src="./assets/js/bulma-carousel.min.js"></script>
  <script src="./assets/js/bulma-slider.min.js"></script>
  <script src="./assets/js/index.js"></script>
  <script src="./assets/js/jquery.event.move.js"></script>
  <script src="./assets/js/jquery.twentytwenty.js"></script>
  <script>
  $(function(){
    $(".twentytwenty-container").twentytwenty();
    // $(".twentytwenty-container", "#results-carousel").twentytwenty({default_offset_pct: 0.5, ratio: 0.5});
  });
  </script>
</body>
</html>



