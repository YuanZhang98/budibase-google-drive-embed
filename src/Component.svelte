<script>
  import { getContext } from "svelte"

  export let width = 640;
  export let height = 480;

  const { styleable } = getContext("sdk")
  const component = getContext("component")
  export let url = '';

  const getIdFromUrl = (url) => {
    const idWithExtraJunk = url.split('/')[5] ?? '';
    return idWithExtraJunk.split('?')[0] ?? '';
  };
  const embedUrl = (id, type) => {
    const embedFolder = (id) => `https://drive.google.com/embeddedfolderview?id=${id}#list`;
    const embedFile = (id) => `https://drive.google.com/file/d/${id}/preview`;

    if (type === 'folder') {
      return embedFolder(id);
    }
    return embedFile(id);
  }

  function checkUrlValid(urlWithPreview) {
    return !!(urlWithPreview.includes("https://drive.google.com/drive/folders/")
            || urlWithPreview.includes("https://drive.google.com/file/"));
  }

  $: isValid = checkUrlValid(url);
  $: fileType = url.includes('folders') ? 'folder' : 'file';
  $: urlAsEmbedLink = embedUrl(getIdFromUrl(url), fileType);
</script>

<div use:styleable={$component.styles}>
  {#if !url}
    <p>Please enter a google drive URL.</p>
  {:else if isValid}
    <iframe src={urlAsEmbedLink} width={width} height={height} allow="autoplay"></iframe>
  {:else}
    <p>Error - Please enter a valid Google Drive url</p>
  {/if}
</div>