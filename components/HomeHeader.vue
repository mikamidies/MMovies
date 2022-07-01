<template>
  <div class="wrap">
    <div class="container content">
      <p class="suptitle">by mikamiDies</p>

      <h1 class="title">MFilms</h1>
      <a href="#grid" class="btn">View now</a>
    </div>
  </div>
</template>

<script>
export default {}
</script>

<style scoped>
.wrap {
  height: 100%;
  min-height: 50vh;
  background-image: url('@/assets/img/home.jpg');
  background-position: center;
  background-repeat: no-repeat;
  background-size: cover;
  position: relative;
  display: flex;
  align-items: center;
}
.wrap::after {
  content: '';
  position: absolute;
  height: 100%;
  width: 100%;
  top: 0;
  left: 0;
  background: rgba(0, 0, 0, 0.6);
}
.content {
  position: relative;
  z-index: 9;
}
.suptitle {
  color: var(--red);
  font-size: 2rem;
  font-weight: 500;
  font-family: var(--regular-sec);
  margin-bottom: 1rem;
}
.title {
  margin-bottom: 2rem;
  font-size: 6rem;
  font-weight: 500;
  font-family: var(--decor);
  letter-spacing: 2px;
}
.btn {
  padding: 0.5rem;
  background: var(--red);
  color: white;
  width: 180px;
  font-size: 18px;
  transition: 0.4s;
  border-radius: 0;
}
.btn:focus,
.btn:hover {
  box-shadow: none;
  background: var(--red-dark);
}
</style>
