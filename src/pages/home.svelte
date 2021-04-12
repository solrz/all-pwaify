<Page name="home">
  {#if !config}
  <List>
<!--    <label for="url">Url to open</label>-->
    <ListInput label="url" id="url" type="text" bind:value={url}/>
<!--    <label for="openInNewWindow">openInNewWindow</label>-->
    <ListItem>
      <span>openInNewWindow</span>
      <Toggle label="openInNewWindowurl" id="openInNewWindow" type="checkbox" bind:checked={openInNewWindow}/>
    </ListItem>
    <Button onClick={setupUrl}>Ready</Button>
  </List>
    {@debug openInNewWindow}
  {/if}
  <iframe title="custom iframe" width="100%" height="100%" src={transformUrl}></iframe>
</Page>
<script>
  import {
    Page, Button, Toggle, Input, ListInput, ListItem, List
  } from 'framework7-svelte';
  let config = JSON.parse(localStorage.getItem('config'))
  let url = (config ?? {url:''}).url
  let openInNewWindow = false
  $: transformUrl = (url.includes('http') || url.includes('https'))?url:'https://'+url
  function setupUrl(){
      let config = {
          url, openInNewWindow
      };
      localStorage.setItem('config', JSON.stringify(config))
  }
</script>
