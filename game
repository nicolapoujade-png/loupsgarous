import streamlit as st
import random
import pandas as pd

# --- CONFIGURATION ET TRADUCTION ---
ROLES_FR = {
    "Loup-Garou": "🐺 Tu dois dévorer les villageois sans te faire attraper.",
    "Voyante": "🔮 Tu peux découvrir l'identité d'un joueur chaque nuit.",
    "Le Garde": "🛡️ Tu protèges un joueur chaque nuit contre les loups.",
    "Cupidon": "💘 Tu désignes deux amoureux au début de la partie.",
    "Sorcière": "🧪 Tu as une potion de vie et une potion de mort.",
    "Villageois": "👨‍🌾 Ton but est de débusquer les loups-garous."
}

st.set_page_config(page_title="Loup-Garou Classe", page_icon="🐺", layout="wide")

# Initialisation du stockage des données
if 'deck' not in st.session_state:
    st.session_state['deck'] = []
if 'assigned' not in st.session_state:
    st.session_state['assigned'] = {}

# --- BARRE LATÉRALE : CONFIGURATION ENSEIGNANT ---
with st.sidebar:
    st.title("👨‍🏫 Maître du Jeu")
    password = st.text_input("Code secret GM", type="password")
    
    st.divider()
    nb_eleves = st.number_input("Nombre d'élèves", min_value=4, max_value=40, value=15)
    nb_loups = st.slider("Nombre de Loups-Garous", 1, (nb_eleves // 3), 2)
    
    st.subheader("Rôles spéciaux")
    inc_voyante = st.checkbox("Voyante", value=True)
    inc_garde = st.checkbox("Le Garde (Bodyguard)", value=True)
    inc_cupidon = st.checkbox("Cupidon", value=True)
    inc_sorciere = st.checkbox("Sorcière", value=False)

    if st.button("🎲 GÉNÉRER LES RÔLES"):
        # Construction du deck
        new_deck = ["Loup-Garou"] * nb_loups
        if inc_voyante: new_deck.append("Voyante")
        if inc_garde: new_deck.append("Le Garde")
        if inc_cupidon: new_deck.append("Cupidon")
        if inc_sorciere: new_deck.append("Sorcière")
        
        while len(new_deck) < nb_eleves:
            new_deck.append("Villageois")
            
        random.shuffle(new_deck)
        st.session_state['deck'] = new_deck
        st.session_state['assigned'] = {} # Reset
        st.success("Nouveau jeu prêt !")

# --- ZONE D'AFFICHAGE PRINCIPALE ---
st.title("🏰 Bienvenue au Village de Thiercelieux")

col1, col2 = st.columns([1, 1])

with col1:
    st.header("📱 Espace Élève")
    nom = st.text_input("Ton prénom :").strip()
    
    if not st.session_state['deck']:
        st.info("Attends que ton professeur génère les rôles...")
    elif nom:
        # Attribution ou récupération du rôle
        if nom not in st.session_state['assigned']:
            current_count = len(st.session_state['assigned'])
            if current_count < len(st.session_state['deck']):
                st.session_state['assigned'][nom] = st.session_state['deck'][current_count]
            else:
                st.error("Tous les rôles sont déjà pris !")
        
        if nom in st.session_state['assigned']:
            role = st.session_state['assigned'][nom]
            with st.expander("👉 CLIQUE ICI POUR VOIR TON RÔLE"):
                st.subheader(f"Tu es : {role}")
                st.write(ROLES_FR[role])
                st.caption("Ne montre pas ton écran aux autres !")

with col2:
    # Tableau de bord secret pour l'enseignant
    if password == "1234": # Vous pouvez changer ce code !
        st.header("📋 Tableau de Bord (Secret)")
        if st.session_state['assigned']:
            # Transformer le dictionnaire en tableau pour la lecture
            df = pd.DataFrame(st.session_state['assigned'].items(), columns=['Élève', 'Rôle'])
            st.table(df)
            
            st.write(f"**Progression :** {len(st.session_state['assigned'])} / {len(st.session_state['deck'])} élèves connectés")
        else:
            st.write("Aucun élève n'a encore rejoint.")
    else:
        st.header("🔒 Accès Restreint")
        st.write("Le tableau de bord s'affichera ici une fois le code GM entré dans la barre latérale.")
