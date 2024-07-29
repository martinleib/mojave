<script>
    import { onMount } from "svelte";
    import Welcome from "./Welcome.svelte";
    import Icon from "@iconify/svelte";

    let closedWelcome = JSON.parse(localStorage.getItem("clickedWelcome")) || false;

    let body;
    let welcomeMessage;
    let currentIcon = "";
    let todoItem = "";
    let todoList = [];
    let todoCounter = 1;

    const prefersDarkMode = window.matchMedia("(prefers-color-scheme: dark)");
    const isDark = prefersDarkMode.matches;

    onMount(() => {
        body = document.querySelector("body");
        welcomeMessage = document.querySelector(".welcome-wrapper-content");
        
        setTheme(isDark ? "dark" : "light");

        const storedTodoList = localStorage.getItem("todoList");
        if (storedTodoList) {
            todoList = JSON.parse(storedTodoList);
        }
    });

    function setTheme(theme) {
        if (!closedWelcome) {
            welcomeMessage.classList.remove("bg-dark","bg-light","text-dark","text-light",);
            welcomeMessage.classList.add(`bg-${theme}`,`text-${theme === "dark" ? "light" : "dark"}`,);
        } else {
            const mojaveAppContent = document.querySelector(".mojave-app-content");
            mojaveAppContent.classList.remove("mojave-await-message");
            mojaveAppContent.classList.add("mojave-showing-content");
        }

        body.classList.remove("bg-dark", "bg-light", "text-dark", "text-light");
        body.classList.add(`bg-${theme}`, `text-${theme === "dark" ? "light" : "dark"}`,);
        body.setAttribute("data-bs-theme", theme);

        if (body.classList.contains("bg-dark")) {
            currentIcon = "ph:sun";
        } else if (body.classList.contains("bg-light")) {
            currentIcon = "ph:moon";
        }
    }

    function handleThemeChange(e) {
        const theme = e.matches ? "dark" : "light";
        setTheme(theme);
    }

    const switchThemes = () => {
        const currentTheme = body.getAttribute("data-bs-theme");
        const newTheme = currentTheme === "dark" ? "light" : "dark";
        setTheme(newTheme);
    };

    prefersDarkMode.addEventListener("change", handleThemeChange);

    const addTodoItem = () => {
        if (todoItem === "") return;
        todoList = [...todoList, todoItem];
        todoItem = "";
        localStorage.setItem("todoList", JSON.stringify(todoList));
    };

    const getNumber = () => todoCounter++;

    const removeTodo = (index) => {
        todoList = todoList.filter((_, i) => i !== index);
        localStorage.setItem("todoList", JSON.stringify(todoList));
        todoCounter--;
    };

    const clearList = () => {
        if (confirm("Are you sure you want to clear the list?")) {
            todoList = [];
            todoCounter = 1;
            localStorage.removeItem("todoList");
        }
    };

    $: isTodoListEmpty = todoList.length === 0;
</script>

{#if !closedWelcome}
    <Welcome />
{/if}

<div class="mojave-app-content mojave-await-message">
    <header class="container pt-2 pb-2 d-flex justify-content-between align-items-center">
        <h1 class="fs-5 mt-0 mb-0">mojave</h1>
        <!-- svelte-ignore a11y-no-noninteractive-element-interactions -->
        <!-- svelte-ignore a11y-click-events-have-key-events -->
        <p class="colorSwitch mb-0" on:click={switchThemes}>
            <Icon icon={currentIcon} />
        </p>
    </header>

    <main>
        <div class="container text-center mojave-app-todo">
            <div class="mt-4">
                <form on:submit|preventDefault={addTodoItem}>
                    <div class="form-floating">
                        <input
                            id="todoItem"
                            type="text"
                            class="form-control"
                            placeholder="..."
                            bind:value={todoItem}
                        />
                        <label for="todoItem">Add an item to the list</label>
                    </div>
                    <input
                        type="submit"
                        class="form-control mt-1"
                        value="Add item"
                    />
                    <input
                        type="button"
                        class="form-control mt-1 clearListButton"
                        value="Clear list"
                        on:click={clearList}
                        disabled={isTodoListEmpty}
                    />
                </form>
            </div>
            <ul class="list-unstyled text-start mt-3">
                {#each todoList as item, index}
                    <!-- svelte-ignore a11y-no-noninteractive-element-interactions -->
                    <!-- svelte-ignore a11y-click-events-have-key-events -->
                    <li on:click={() => removeTodo(index)}>
                        {getNumber()}. <span class="todoListItem">{item}</span>
                    </li>
                {:else}
                    <p class="mb-0">
                        There are no pending tasks! Guess you'll be having a
                        chill day today then, huh.
                    </p>
                {/each}
            </ul>
        </div>
    </main>
</div>
