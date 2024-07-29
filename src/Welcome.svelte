<script>
    let closedWelcome = false;

    const handleClosingWrapper = () => {
        const welcomeWrapper = document.querySelector(".welcome-wrapper");
        welcomeWrapper.classList.remove("d-flex");
        welcomeWrapper.classList.add("d-none");

        const mojaveAppContent = document.querySelector(".mojave-app-content");
        mojaveAppContent.classList.remove("mojave-await-message");
        mojaveAppContent.classList.add("mojave-showing-content");
    
        closedWelcome = true;
        localStorage.setItem("clickedWelcome", JSON.stringify(closedWelcome));
    };
</script>

<div class="welcome-wrapper container-fluid d-flex align-items-center justify-content-center">
    <div class="welcome-wrapper-content text-center p-4">
        <h1 class="fs-4 mb-0">Hey there,</h1>
        <h2 class="fs-3 mt-0">welcome to Mojave, a to-do list app.</h2>

        <p class="fs-6 mt-4 mb-0">
            To get started, add an item to the list using the form below.
        </p>
        <p class="fs-6 mt-0 mb-0">
            You can click on an item to remove it from the list.
        </p>
        <p class="fs-6 mt-0 mb-0">
            You can also clear the list by clicking the "Clear list" button.
        </p>

        <p class="fs-6 mt-4 mb-0 fw-bold">
            Don't worry: your list will be saved even if you close the tab or
            refresh the page.
        </p>
        <form
            action="#"
            class="mt-3"
            on:submit|preventDefault={handleClosingWrapper}
        >
            <input type="submit" class="form-control" value="Got it" />
        </form>
    </div>
</div>
