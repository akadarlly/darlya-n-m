import streamlit as st
import math

st.set_page_config(page_title="Sac Büküm Uzaması", page_icon="📐")
st.title("📐 Sac Büküm Uzaması (Bend Allowance) Hesaplama")

st.markdown("""
Bu uygulama, sac metal büküm işlemlerinde oluşan uzama miktarını hesaplamak için kullanılır.  
**Formül:**  
\\[
BA = (\\theta \\times (R + K \\times t)) \\times \\frac{\\pi}{180}
\\]
""")

if 'material' not in st.session_state:
    st.session_state.material = "DKP"
if 'manual_k' not in st.session_state:
    st.session_state.manual_k = 0.33

thickness = st.number_input("Sac Kalınlığı (t) [mm]", min_value=0.01, value=2.0, step=0.1)
radius = st.number_input("İç Yarıçap (R) [mm]", min_value=0.01, value=1.0, step=0.1)
angle = st.number_input("Büküm Açısı (θ) [derece]", min_value=0.0, max_value=360.0, value=135.0, step=1.0)

material = st.selectbox("Malzeme Tipi", ["DKP", "Paslanmaz", "Alüminyum", "Manuel Giriş"],
                        index=["DKP", "Paslanmaz", "Alüminyum", "Manuel Giriş"].index(st.session_state.material))

if material == "DKP":
    k_factor = 0.38
elif material == "Paslanmaz":
    k_factor = 0.4
elif material == "Alüminyum":
    k_factor = 0.35
else:
    k_factor = st.number_input("K-Faktörü", min_value=0.0, max_value=0.5, value=st.session_state.manual_k, step=0.01)
    st.session_state.manual_k = k_factor

st.session_state.material = material

bend_allowance = (angle * (radius + k_factor * thickness)) * math.pi / 180
st.success(f"Hesaplanan Uzama (BA): **{bend_allowance:.2f} mm**")
