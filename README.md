# Paris-GP1-bousselmi-afriat

GP1 / name 1 / name 2

import pandas as pd
import plotly.express as px
import seaborn as sns
import matplotlib.pyplot as plt

# Charger les données de températures et les vérifier
df = pd.read_csv("/content/CLIMAT RISK EXCEL.xlsx")
df.dtypes
df.head()
df.info()
df.isnull().sum()
df.columns
df.describe()

# Réorganiser les données
df = df.rename(columns={'Country':'country', 'cri_score':'score'})
df.head(33)

# Créer une carte choroplèthe
fig = px.choropleth(data_frame=df,
                    locations="country",
                    locationmode="country names",
                    color="score",
                    scope="world")
fig.show()

# Exporter la carte en format HTML
fig.update_layout(title_text="Risques climatiques et pertes économiques",
                  coloraxis_colorbar_title="score")
fig.write_html("europe.html", auto_open=True)

# Sélectionner les dix pays les moins risqués
df_top = df[(df["score"] > 12.17) & (df["score"] < 26)]
df_top = df_top.sort_values(by='score', ascending=True)

# Créer un graphique à barres pour visualiser les résultats
sns.barplot(x="score", y="country", data=df_top, palette="crest")
plt.title("10 pays les moins risqués selon l'indice de risque climatique")
plt.show()
