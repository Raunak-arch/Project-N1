https://exoplanetarchive.ipac.caltech.edu/TAP/sync?query=select+pl_name,hostname,discoverymethod,disc_year,sy_pnum,sy_snum,pl_orbper,pl_orbsmax,pl_rade,pl_radj,pl_bmasse,pl_bmassj,pl_orbeccen,pl_eqt,pl_insol,st_teff,st_rad,st_mass,st_met,st_logg,sy_dist,sy_vmag+from+pscomppars&format=csv
used this for Downloding databse
df = pd.read_csv(r"C:\Users\DELL\Downloads\data.csv")
df = df.fillna('Unknown')
pl_name               0
hostname              0
discoverymethod       0
disc_year             0
sy_pnum               0
sy_snum               0
pl_orbper           353
pl_orbsmax          428
pl_rade              50
pl_radj              50
pl_bmasse            31
pl_bmassj            31
pl_orbeccen        1066
pl_eqt              536
pl_insol            592
st_teff             307
st_rad              331
st_mass               9
st_met              650
st_logg             336
sy_dist              28
sy_vmag             312
import pymysql
import sqlalchemy
from sqlalchemy impot create_engine
engine = create_engine("mysql+pymysql://root:12345@localhost:3306/analysis")
df.to_sql('PlanetData',engine,if_exists='replace',index=False)
df1=pd.read_sql_query('Select * From PlanetData',engine)
df12=df12.astype(float)
