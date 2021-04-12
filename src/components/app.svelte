<App { ...f7params } >
  <View main class="safe-areas" url="/" />
</App>
<script>
  import { onMount } from 'svelte';

  import {
    f7,
    f7ready,
    App,
    Panel,
    Views,
    View,
    Popup,
    Page,
    Navbar,
    Toolbar,
    NavRight,
    Link,
    Block,
    BlockTitle,
    LoginScreen,
    LoginScreenTitle,
    List,
    ListItem,
    ListInput,
    ListButton,
    BlockFooter
  } from 'framework7-svelte';


  import routes from '../js/routes';
  import store from '../js/store';


  // Framework7 Parameters
  let f7params = {
    name: 'AllPwaify', // App name
    theme: 'auto', // Automatic theme detection



    // App store
    store: store,
    // App routes
    routes: routes,
  };

  onMount(() => {
    f7ready(() => {
        let config = JSON.parse(localStorage.getItem('config'))
        if(config){
            console.log('Got config')
            if(config.openInNewWindow){
                console.log('Move to url')
                const url = config.url
                const transformUrl = (url.includes('http') || url.includes('https'))?url:'https://'+url
                window.open(transformUrl, '_self')
            }
        }

      // Call F7 APIs here
    });
  })
</script>
