<template>
  <div class="container">
    <h2>Alors,<br />On prend RDV ?</h2>
    <form @submit="checkForm">
      <label for="nom"
        >Votre nom* ...
        <input v-model="nom" type="text" name="nom" id="nom" required />
      </label>
      <label for="prenom">
        Votre prénom* ...
        <input
          v-model="prenom"
          type="text"
          name="prenom"
          id="prenom"
          required
        />
      </label>
      <label for="email">
        Votre email* ...
        <input v-model="email" type="email" name="email" id="email" required />
      </label>
      <label for="consent">
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
        <a href="mailto:contact@btg-communication.fr"
          >contact@btg-communication.fr</a
        >
        Plus d’informations sur notre politique de confidentialité.
      </label>
      <input
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
      key: '6LcxmdgUAAAAAJBTLo4dFvRVh1wpxcBxJJqWjixH',
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
        bodyFormData.set('sitekye', this.key)

        axios({
          method: 'post',
          url: 'http://btg-communication.local//wp-json/contact-form-7/v1/contact-forms/1460/feedback',
          data: bodyFormData,
          config: { headers: { 'Content-Type': 'multipart/form-data' } },
        })
          .then((response) => {
            console.log(response)
            this.sent = true
            return true
          })
          .catch((error) => console.log(error))
      }
    },
  },
}
</script>
