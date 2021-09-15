<template>
  <div class="container">
    <h2 class="contact-title">
      Alors,<br /><span class="bold">On prend RDV ?</span>
    </h2>
    <form id="audit" @submit="checkForm">
      <label for="nom"
        ><span class="screen-reader-text">Votre nom* ... </span>
        <input
          v-model="nom"
          type="text"
          name="nom"
          id="nom"
          placeholder="Votre nom* ..."
          required
        />
      </label>
      <label for="prenom">
        <span class="screen-reader-text">Votre prénom* ... </span>
        <input
          v-model="prenom"
          type="text"
          name="prenom"
          id="prenom"
          placeholder="Votre prenom* ..."
          required
        />
      </label>
      <label for="email">
        <span class="screen-reader-text">Votre email* ... </span>
        <input
          v-model="email"
          type="email"
          name="email"
          id="email"
          placeholder="Votre email* ..."
          required
        />
      </label>
      <label class="consent" for="consent">
        <input
          v-model="consent"
          type="checkbox"
          name="consent"
          id="consent"
          required
        />
        Les informations recueillies sur ce formulaire sont enregistrées dans un
        fichier informatisé par Btg communication pour la gestion des demandes
        et de l’utilisation du site. Elles sont conservées pendant 5 ans après
        la fin des relations contractuelles. Conformément à la loi «
        informatique et libertés », vous pouvez exercer votre droit d’accès aux
        données vous concernant et les faire rectifier en nous contactant via :
        <a class="nostyle" href="mailto:contact@btg-communication.fr"
          >contact@btg-communication.fr</a
        ><br />
        Plus d’informations sur notre
        <nuxt-link to="politique">politique de confidentialité.</nuxt-link>
      </label>
      <input
        class="submit"
        type="submit"
        value="Envoyer ma demande d'audit"
        aria-label="Envoyer ma demande d'audit à BTG communication"
      />
    </form>
    <section class="afterform">
      <p>
        À l'issue de cet audit vous aurez eu l'occasion de mieux nous connaitre
        et de découvrir notre approche.
      </p>
      <p>
        Si elle fait écho à vos envies et besoins
        <span class="bold"
          >nous serions très heureux de mettre en place nos recommandations avec
          vous.</span
        >
      </p>
    </section>
  </div>
</template>
<script>
import axios from 'axios'
export default {
  name: 'ContactForm',
  data() {
    return {
      nom: null,
      prenom: null,
      email: null,
      consent: false,
      sent: false,
    }
  },
  methods: {
    validEmail(email) {
      const re =
        /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/
      return re.test(email)
    },

    checkForm(e) {
      e.preventDefault()
      this.errors = []

      if (!this.nom) {
        this.errors.push('Veuillez saisir votre nom.')
      }
      if (!this.email) {
        this.errors.push('Email requis.')
      } else if (!this.validEmail(this.email)) {
        this.errors.push('Un email valide est requis.')
      }
      if (!this.prenom) {
        this.errors.push('Veuillez saisir votre prénom.')
      }

      if (!this.errors.length) {
        const bodyFormData = new FormData()
        bodyFormData.set('nom', this.nom)
        bodyFormData.set('prenom', this.prenom)
        bodyFormData.set('email', this.email)
        bodyFormData.set('consent', this.consent)
        bodyFormData.set('sitekye', this.key)

        axios({
          method: 'post',
          url: 'http://btg-communication.local/wp-json/contact-form-7/v1/contact-forms/1460/feedback',
          data: bodyFormData,
          config: { headers: { 'Content-Type': 'multipart/form-data' } },
        })
          .then((response) => {
            console.log(response)
            this.sent = true
            this.nom = null
            this.prenom = null
            this.email = null
            this.consent = false
            return true
          })
          .catch((error) => console.log(error))
      }
    },
  },
}
</script>
