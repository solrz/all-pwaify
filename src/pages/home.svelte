<Page name="home">
  {#if !config || !isPWA}
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
  <iframe height="100%" src={transformUrl} title="custom iframe" width="100%"></iframe>
</Page>
<script>
    import {
        Page, Button, Toggle, Input, ListInput, ListItem, List, f7
    } from 'framework7-svelte';
    import {onMount} from "svelte";

    let config = JSON.parse(localStorage.getItem('config'))
    let url = (config ?? {url: ''}).url
    let openInNewWindow = false
    $: transformUrl = (url.includes('http') || url.includes('https')) ? url : 'https://' + url
    $: isPWA = navigator.standalone || window.matchMedia('(display-mode: standalone)').matches
    const jdenticon = require("jdenticon");

    const value = "icon value";
    const size = 200;

    onMount(function () {
        if (navigator.permissions) {
            navigator.permissions.query({name: "clipboard-read"}).then(result => {
                // If permission to read the clipboard is granted or if the user will
                // be prompted to allow it, we proceed.

                if (result.state == "granted" || result.state == "prompt") {
                    setTimeout(async () => {
                        let clipboardText = await window.navigator.clipboard.readText();
                        url = clipboardText
                        console.log(clipboardText)
                    }, 1)
                }
            });
        }else{
            var interacted = false;
            function fun(){
                interacted = true;
                window.removeEventListener('scroll',fun)
                window.removeEventListener('click',fun)
                play();
            }
            window.addEventListener('scroll',fun)
            window.addEventListener('click',fun)
            function play(){
                if(interacted){
                    setTimeout(async () => {
                        let clipboardText = await window.navigator.clipboard.readText();
                        url = clipboardText
                        console.log(clipboardText)
                    }, 1)
                }
            }
        }
    })

    function setupUrl() {
        let config = {
            url, openInNewWindow
        };
        localStorage.setItem('config', JSON.stringify(config))
        f7.dialog.alert('', 'Url setup.');
        const web_favicon = jdenticon.toPng(url, 512);
        const ios_favicon = jdenticon.toPng(url, 256);
        document.getElementById('src-apple-touch-icon').setAttribute('href', URL.createObjectURL(new Blob([ios_favicon])))
        document.getElementById('src-icon').setAttribute('href',URL.createObjectURL(new Blob([ios_favicon])))
    }

    function getDomain(url, subdomain) {
        subdomain = subdomain || false;

        url = url.replace(/(https?:\/\/)?(www.)?/i, '');

        if (!subdomain) {
            url = url.split('.');

            url = url.slice(url.length - 2).join('.');
        }

        if (url.indexOf('/') !== -1) {
            return url.split('/')[0];
        }

        return url;
    }
</script>
