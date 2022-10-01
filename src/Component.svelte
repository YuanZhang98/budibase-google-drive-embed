<script>
  import { getContext } from "svelte"

  export let text

  export let width = 640;
  export let height = 480;

  const { styleable } = getContext("sdk")
  const component = getContext("component")
  export let url;
  let src;
  let urlWithPreview;

  function changeUrl(url) {
    if (url.match("https://drive.google.com/drive/folders/")) {
      urlWithPreview = url.replace("drive/folders/", "embeddedfolderview?id=")
              .replace("?usp=sharing", "#list")
    } else {
      urlWithPreview = url.replace("/view?usp=sharing", "/preview");
    }
    return urlWithPreview
  }

  $: src = changeUrl(url);
</script>

<div use:styleable={$component.styles}>
  <iframe src={src} width={width} height={height} allow="autoplay"></iframe>
</div>