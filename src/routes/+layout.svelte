<!--Este layout se aplica a todas las paginas-->
<!--Que se encuentren en este directorio-->
<script lang="ts">
	// logic goes here
    //Import assets
    import {base} from '$app/paths';
    //global "app.css"
    import "../app.css";
    let author = `${base}/assets/logos/author/EJ-Logo-2-compressed.svg`;
    // Add state for menu
    let isMenuOpen = false;
    let isFooterVisible = false;
    let isMenuFullyOpen = false;
    let isScrolling = false;
    let isScrollButtonVisible = false;

    // Toggle menu function
    const toggleMenu = () => {
        isMenuOpen = !isMenuOpen;
        
        if (isMenuOpen) {
            // Wait for menu animation to complete before showing cursor
            setTimeout(() => {
                isMenuFullyOpen = true;
            }, 250); // Match this with your transition time
        } else {
            isMenuFullyOpen = false;
        }
    }

    // Close menu when clicking on opacity filter
    const handleOverlayClick = () => {
        if (isMenuOpen) {
            // Find and trigger the reset button
            const resetButton = document.querySelector('.cancel_search_button');
            if (resetButton) {
                (resetButton as HTMLButtonElement).click();
            }
            isMenuOpen = false;
            isMenuFullyOpen = false;
        }
    }

    // Add scroll handler
    function handleScroll() {
        const scrollHeight = document.documentElement.scrollHeight;
        const scrollTop = window.scrollY;
        const clientHeight = document.documentElement.clientHeight;
        const header = document.querySelector('header');
        
        // Show button after scrolling past header
        isScrollButtonVisible = header ? scrollTop > header.offsetHeight : false;
        
        // Calculate scroll percentage for footer
        const scrollPercentage = (scrollTop + clientHeight) / scrollHeight;
        isFooterVisible = scrollPercentage > 0.95;
    }

    // Add onMount to set up scroll listener
    import { onMount } from 'svelte';
    
    onMount(() => {
        window.addEventListener('scroll', handleScroll);
        return () => window.removeEventListener('scroll', handleScroll);
    });

    const scrollToTop = () => {
        isScrolling = true;
        window.scrollTo({ top: 0, behavior: 'smooth' });
        // Reset after animation
        setTimeout(() => isScrolling = false, 500);
    }

</script>

<!-- markup (zero or more items) goes here-->
    <!--HEADER-->
    <div class="page_wrapper" class:menu-open={isMenuOpen}>
    <div class="opacity_filter" 
         class:active={isMenuOpen}
         class:cursor-active={isMenuFullyOpen}
         style="--cursor-url: url('{base}/assets/icons/cursor/cursor_opacity.svg')"
         on:click={handleOverlayClick}
         on:keydown={e => e.key === 'Escape' && handleOverlayClick()}
         role="button"
         tabindex="-1"
         aria-label="Close menu overlay"></div>
        <header>
            <nav>
                <a id="Logo" href="#">
                    <img src={author} alt="EJ">
                </a>
                <button class="nav_button" on:click={toggleMenu} type="button" aria-expanded={isMenuOpen} aria-label="menu">
                    <i class='bx bx-menu bx-lg'></i>
                </button>
            </nav>
            <div class="mobile_menu" class:open={isMenuOpen}>
                <div class="mobile_menu_content">
                    <ul class="main_list">
                        <li><a href="#">Home</a></li>
                        <li><a href="#">Elian</a></li>
                        <li><a href="#">Portfolio</a></li>
                        <li><a href="#">Contact</a></li>
                    </ul>
                    <button class="nav_close"
                        on:click={toggleMenu}
                        type="button"
                        aria-label="Close menu">
                        <i class='bx bx-x bx-md'></i>
                    </button>
                    <div class="search_container">
                        <form class="search_form">
                            <input class="search_input" 
                                   type="text" 
                                   name="Buscar" 
                                   autocomplete="off" 
                                   placeholder="Search...">
                            <button class="cancel_search_button" 
                                    type="reset" 
                                    aria-label="Close search">
                                <i class='bx bx-x'></i>
                            </button>
                            <button class="search_button" type="submit" aria-label="Search">
                                <text>GO</text>
                            </button>
                        </form>
                    </div>
                </div>
            </div>
        </header>
        <main class="content">
            <slot />
        </main>
        <button class="scroll_up_button" 
                class:visible={isScrollButtonVisible}
                on:click={scrollToTop}
                on:keydown={e => e.key === 'Enter' && scrollToTop()}
                type="button"
                aria-label="Scroll to top">
            <i class='bx bx-chevron-up bx-md' class:bx-fade-up={isScrolling}></i>
        </button>
        <footer class:visible={isFooterVisible}>
            <div class="footer_content">
                <p>© Copyright - Elian Javier - 2025. All rights reserved</p>
            </div>
        </footer>
    </div>
<style>

    .page_wrapper {
        position: relative;
        left: 0;
        /*Adding fix to footer*/
        min-height: 100vh;
        display: grid;
        grid-template-rows: auto 1fr auto;
        /*End of fix--review this*/
        -webkit-transition: left 0.4s ease;
        -moz-transition: left 0.4s ease;
        -o-transition: left 0.4s ease;
        transition: left 0.4s ease;
    }    

    .page_wrapper.menu-open {
        left: -10%;
    }

    .opacity_filter {
        position: fixed;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        background: rgba(0, 0, 0, 0.3);
        z-index: 98;
        opacity: 0;
        visibility: hidden;
        -webkit-transition: opacity 0.4s ease, visibility 0.4s ease;
        -moz-transition: opacity 0.4s ease, visibility 0.4s ease;
        -o-transition: opacity 0.4s ease, visibility 0.4s ease;
        transition: opacity 0.4s ease, visibility 0.4s ease;
    }

    .opacity_filter.active {
        opacity: 1;
        visibility: visible;
    }

    .opacity_filter.cursor-active {
        cursor: var(--cursor-url), auto;
    }

    /*Adding fix to footer*/
    header {
        grid-row: 1;
    }

    :global(slot) {
        grid-row: 2;
    }
    /*End of fix--review this*/

    nav {
        max-width: 85%;
        /*How can I make this margins responsive?*/
        margin-inline: auto;
        margin-top: clamp(2rem, 1vw, 3rem);
        margin-bottom: clamp(0.5rem, 1vw, 1rem);
        display: grid;
        grid-template-columns: repeat(2, minmax(100px, 1fr));
        align-items: center;
    }
    
    #Logo {
        max-width: fit-content;
    }
    
    #Logo img {
        height: 2.5rem;
    }

    .nav_button {
        justify-self: end;
        padding: 0;
    }

    .nav_button .bx-menu {
        transform: translateX(5px);
        padding: 0.3rem;
    }

    .nav_close {
        padding: 0.3;
        margin-left: 1.7rem;
        grid-column: 3;
    }

    .mobile_menu_content {
        margin-top: 3rem;
        display: grid;
        grid-template-columns: repeat(3, 1fr);
        grid-template-rows: repeat(auto-fit, minmax(20px, 100px));
        align-items: start;
    }

    .mobile_menu_content > * {
        max-height: fit-content;
    }

    .mobile_menu {
        z-index: 99;
        background-color: hsl(0, 0%, 10%);
        position: fixed;
        width: clamp(250px, 35vw, 400px);
        top: 0;
        right: -100%;
        height: 100vh;
        overflow-y: auto;
        overflow-x: hidden;
        -webkit-transition: right 0.4s ease;
		-moz-transition: right 0.4s ease;
		-o-transition: right 0.4s ease;
		transition: right 0.4s ease;
    }

    .mobile_menu.open {
        right: 0;
    }

    .main_list {
        display: inline-block;
        grid-column: 1 / span 2;
        grid-row: 2;
    }

    .main_list li {
        display: flex;
        justify-content: center;
        padding: .5rem 0;
    }

    .main_list li a {
        min-width: 7ch;
        padding: 5px 0;
    }

    .main_list li a:hover {
        color: hsl(0, 0%, 70%);
    }

    .search_container {
        grid-column: 1 / span 3;
        margin-top: 3rem;
        display: grid;
        grid-template-columns: 1fr;
        grid-template-rows: max-content;
    }

    .search_form {
        width: 100%;
        place-self: center;
        position: relative;
        text-align: center;
    }
    .search_form > * {
        height: 2.1rem;
        vertical-align: middle;
    }

    .search_input {
        width: 45%;
        color: hsl(0, 0%, 0%);
        padding: 0.5rem;
        border: none;
        border-radius: 0.25rem 0 0 0.25rem;
        background-color: hsl(0, 0%, 100%);
        -webkit-transition: width 0.1s ease-in-out 200ms;
        -moz-transition: width 0.1s ease-in-out 200ms;
        -o-transition: width 0.1s ease-in-out 200ms;
        transition: width 0.1s ease-in-out 200ms;
    }

    .search_input:focus {
        outline-color: transparent;
        width: 60%;
    }

    .search_input::placeholder {
        color: hsl(0, 0%, 5%);
    }

    .search_form button, .search_form input {
        font-weight: 700;
    }

    .search_form button > * {
        line-height: 1;
        vertical-align: middle;
    }

    .search_button {
        background-color: hsl(0, 0%, 100%);
        border-radius: 0 0.25rem 0.25rem 0;
        padding: 0.5rem;
        color: hsl(0, 0%, 0%);
    }

    .search_button > * {
        line-height: 1.2 !important;
    }

    .search_input:not(:placeholder-shown) + .cancel_search_button {
        visibility: visible;
    }

    .search_input:not(:focus) + .cancel_search_button {
        right: calc(27.5% + 1.2rem);
    }

    .cancel_search_button {
        color: hsl(0, 0%, 0%);
        right: calc(20% + 1.2rem);
        border: transparent 0 none;
	    background: hsl(0, 0%, 100%);
        position: absolute;
        padding: 0;
        outline-color: transparent;
        visibility: hidden;
        -webkit-transition: right 0.1s ease-in-out 200ms, visibility 0.1s ease;
        -moz-transition: right 0.1s ease-in-out 200ms, visibility 0.1s ease;
        -o-transition: right 0.1s ease-in-out 200ms, visibility 0.1s ease;
        transition: right 0.1s ease-in-out 200ms, visibility 0.1s ease;
    }

    .cancel_search_button .bx-x {
        font-size: 1.2rem;
    }

    .scroll_up_button {
        position: fixed;
        bottom: 10.7rem;
        right: 10vw;
        z-index: 2;
        background-color: hsl(0, 0%, 90%);
        border-radius: .25rem;
        padding: 0.25rem;
        opacity: 0;
        visibility: hidden;
        transform: scale(0.5);
        transition: opacity 0.2s ease,
                    visibility 0.2s ease,
                    transform 0.2s ease,
                    bottom 1s ease,
                    right 1s ease;
        @media only screen and (min-width: 992px) {
            bottom: 6.4rem;
            right: 2.3vw;
        }
    }

    .scroll_up_button.visible {
        opacity: 1;
        visibility: visible;
        transform: scale(1);
    }

    .bx-chevron-up {
        color: hsl(0, 0%, 0%);
    }

    footer {
        position: relative;
        width: 100%;
        grid-row: 3;
        opacity: 0;
        transform: translateY(20px);
        -webkit-transition: opacity 0.4s ease, transform 0.4s ease;
        -moz-transition: opacity 0.4s ease, transform 0.4s ease;
        -o-transition: opacity 0.4s ease, transform 0.4s ease;
        transition: opacity 0.4s ease, transform 0.4s ease;
    }

    footer.visible {
        opacity: 1;
        transform: translateY(0);
    }

    .footer_content {
        margin: 4.5rem auto 2rem auto;
        width: 85%;
    }

    .footer_content p {
        vertical-align: middle;
    }

    @media(hover: hover) {
        .scroll_up_button:hover {
            background-color: hsl(0, 0%, 75%);
        }
    }
    
</style>
