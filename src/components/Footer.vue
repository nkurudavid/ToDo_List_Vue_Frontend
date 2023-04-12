<template>
    <footer class="row m-0 bg-light">
        <div class="py-3 my-4">
            <div class="container">
                <ul class="nav justify-content-center border-bottom pb-3 mb-3">
                    <li class="nav-item">
                        <RouterLink :class="'nav-link px-2 text-muted'" to="/">Home</RouterLink>
                    </li>
                    <li class="nav-item">
                        <RouterLink :class="'nav-link px-2 text-muted'" to="/about">About</RouterLink>
                    </li>
                </ul>
                <p class="text-center text-muted">&copy; 2023 Vuengo ToDo App</p>
            </div>
        </div>
    </footer>
</template>
