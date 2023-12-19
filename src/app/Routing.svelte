<script>
  import { onMount, setContext } from "svelte";
  import { writable } from "svelte/store";
  import Home from "../pages/Home.svelte";
  import Page2 from "../pages/Page2.svelte";
  import Page1 from "../pages/Page1.svelte";
  import PageNotFound from "../pages/PageNotFound.svelte";
  
    const currentRoute = writable("");

    setContext("route", currentRoute)

    const routes = [
        {
            link: "/",
            element: Home,
        },
        {
            link: "/page1",
            element: Page1,
        },
        {
            link: "/page2",
            element: Page2
        },
        {
            link: "/error",
            element: PageNotFound
        }
    ]

    const navigate = (route) => {
        $currentRoute = route;
        window.history.pushState({}, '', route);
    };

    const checkForRoute = () => {
        let error = false;
        for(let el of routes){
            console.log(el)
            if(window.location.pathname == el.link){
                navigate(el.link)
                error = false
                return error 
            }else{
                error = true
            }
        }
        return error
    }



    onMount(() => {
        checkForRoute() ? navigate("/error") : navigate(window.location.pathname)
    });


</script>

<!-- svelte-ignore a11y-click-events-have-key-events -->
<div>
    <nav>
      <a on:click={() => navigate('/')}>Home</a>
      <a on:click={() => navigate('/page1')}>1</a>
      <a on:click={() => navigate('/page2')}>2</a>
    </nav>
  

    {#each routes as route }
        {#if route.link == $currentRoute}
            <svelte:component this={route.element} />
        {/if}
    {/each}
  </div>

