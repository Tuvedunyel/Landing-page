<template>
  <header>
    <div class="container">
      <img
        src="../static/img/blackbullet-right.png"
        alt="background-image"
        id="leftimage"
      />
      <img
        src="../static/img/blackbullet-left.png"
        alt="background-image"
        id="rightimage"
      />
      <img
        src="../static/img/logodeux.svg"
        alt="Logo de BTG Communication"
        class="logo"
      />
      <div class="title-container">
        <h1><span>Vous souhaitez faire un point sur vos actions</span></h1>
        <h1><span>Et améliorer votre communication ?</span></h1>
        <ul class="competences">
          <li>Stratégie</li>
          <li>Marketing</li>
          <li>Positionnement</li>
          <li>Identité</li>
          <li>Print</li>
          <li>Web</li>
          <li>Vidéo</li>
        </ul>
      </div>
      <a class="audit" href="#audit"
        >Programmer mon audit <span class="yellow">offert</span></a
      >
      <p>Valeur : 800€ ht</p>
    </div>
  </header>
</template>

<script>
export default {
  name: 'HeroBanner',
}
</script>
