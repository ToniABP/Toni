#ABP

--------------------MENU PRINCIPAL--------------------------------------

using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.IO;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;

namespace WindowsFormsApp1
{
    public partial class Form1 : Form
    {
        List<Personajes> personajes = new List<Personajes>();
        List<Preguntes1> preguntes = new List<Preguntes1>();
        string idioma = "";

        public Form1()
        {
            InitializeComponent();
  

        }

        private void Form1_Load(object sender, EventArgs e)
        {
            dataGridViewpersonatges.AutoGenerateColumns = false;
        }

        private void buttonafegirper_Click(object sender, EventArgs e)
        {
            Afegirpersonatges ap = new Afegirpersonatges(personajes);
            ap.ShowDialog();
            //Muestra la lista en el datagrid
            dataGridViewpersonatges.DataSource = null;
            dataGridViewpersonatges.DataSource = personajes;

        }

        private void buttonmodificarper_Click(object sender, EventArgs e)
        {
            Modificar_personatges mp = new Modificar_personatges();
            mp.ShowDialog();
        }

        private void buttonafegirpreg_Click(object sender, EventArgs e)
        {
            Form2 apr = new Form2();
            apr.ShowDialog();

        }

        private void buttonmodificarpreg_Click(object sender, EventArgs e)
        {
            Modificarpreguntes mpr = new Modificarpreguntes();
            mpr.ShowDialog();
        }

        private void buttoneliminarper_Click(object sender, EventArgs e)
        {

            if (idioma == "cat")
            {


                if (this.dataGridViewpersonatges.SelectedRows.Count > 0)
                {
                    //Obtener la fila seleccionada y guaradarla en un objeto de tipo Personajes
                    //borrar de la lista el elemento seleccionado
                    JArray jArraypersonatges = (JArray)JToken.FromObject(personajes);

                    StreamWriter arxiuescriptura = File.CreateText(@"..\..\Catala_pers.json");
                    JsonTextWriter escriurearxiu = new JsonTextWriter(arxiuescriptura);

                    personajes.Remove((Personajes)dataGridViewpersonatges.SelectedRows[0].DataBoundItem);
                    dataGridViewpersonatges.DataSource = null;
                    dataGridViewpersonatges.DataSource = personajes;


                    //Muestra la lista en el datagrid
                    //dataGridViewpersonatges.DataSource =

                    //dataGridViewpersonatges.Items.Remove(dataGridViewpersonatges.SelectedItem);
                }
            }
        }

        private void buttoneliminarpreg_Click(object sender, EventArgs e)
        {
            //dataGridViewpreguntes.Item.Remove(dataGridViewpreguntes.SelectedItem);
        }

        private void buttoncatala_Click(object sender, EventArgs e)
        {
            string catala_pers = @"..\..\Catala_pers.json";
            idioma = "cat";
            //Leer el JSON seleccionado y guardarlo en una lista de preguntas
            JArray jArrayPersonatges = JArray.Parse(File.ReadAllText(catala_pers));
            personajes = jArrayPersonatges.ToObject<List<Personajes>>();

            //Muestra la lista en el datagrid
            dataGridViewpersonatges.DataSource = personajes;

           
            string catala_preg = @"..\..\Catala_pre.json";
            //Leer ahora el fichero de preguntas de catala//
            JArray jArrayPreguntes = JArray.Parse(File.ReadAllText(catala_preg));
            preguntes = jArrayPreguntes.ToObject<List<Preguntes1>>();

            dataGridViewpreguntes.DataSource = preguntes;
        }

        private void buttonespaña_Click(object sender, EventArgs e)
        {
            string cas_pers = @"..\..\Castellano_pers.json";
            idioma = "cas";
            JArray jArrayPersonajes = JArray.Parse(File.ReadAllText(cas_pers));
            personajes = jArrayPersonajes.ToObject<List<Personajes>>();

            dataGridViewpersonatges.DataSource = personajes;

            string cas_preg = @"..\..\Castellano_preg.json";
            JArray jArraypreguntas = JArray.Parse(File.ReadAllText(cas_preg));
            preguntes = jArraypreguntas.ToObject<List<Preguntes1>>();

            dataGridViewpreguntes.DataSource = preguntes;
        }

        private void dataGridViewpersonatges_CellContentClick(object sender, DataGridViewCellEventArgs e)
        {

        }

        private void buttonEnglish_Click(object sender, EventArgs e)
        {
            string eng_pers = @"..\..\English_pers.json";
            idioma = "eng";
            JArray jArrayPersonajes = JArray.Parse(File.ReadAllText(eng_pers));
            personajes = jArrayPersonajes.ToObject<List<Personajes>>();

            dataGridViewpersonatges.DataSource = personajes;

            string eng_preg = @"..\..\English_pre.json";
            JArray jArraypreguntas = JArray.Parse(File.ReadAllText(eng_preg));
            preguntes = jArraypreguntas.ToObject<List<Preguntes1>>();

            dataGridViewpreguntes.DataSource = preguntes;
        }
    }
}




----------------------------------AFEGIR PERSONATGES--------------------------------

using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.IO;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;

namespace WindowsFormsApp1
{
    public partial class Afegirpersonatges : Form
    {
        List<Personajes> personajes = new List<Personajes>();
        Personajes personaje1 = new Personajes();

        public Afegirpersonatges(List<Personajes> personajes)
        {
            InitializeComponent();

            this.personajes = personajes;
        }
         
        private void buttonsortir_Click(object sender, EventArgs e)
        {
            this.Close();
        }

        private void button1_Click(object sender, EventArgs e)
        {
            //Se abra ventana seleccionar fichero
            OpenFileDialog BuscarImagen = new OpenFileDialog();
            BuscarImagen.Filter = "Arxiu de imatge|*.*";
           
            BuscarImagen.InitialDirectory = "\\ABP\\WindowsFormsApp1";

            if (BuscarImagen.ShowDialog() == DialogResult.OK)
            {
                //textBoximatge.Text = BuscarImagen.FileName;
                File.Copy(BuscarImagen.FileName, @"..\..\..\imagen\" + BuscarImagen.SafeFileName);

                pictureBox1.ImageLocation = @"..\..\..\imagen\" + BuscarImagen.SafeFileName;
                pictureBox1.SizeMode = PictureBoxSizeMode.StretchImage;


            }

            // C:\dada\pepe.png
            //Copiar este fichero a la ruta de las imagenes
            //pictureBox1.ImageLocation = ;
            //d:\datos\imagenes\pepe.png
            //Picture imagelocation l

            // pictureBox1.ImageLocation = ""
        }

        private void buttonguardar_Click(object sender, EventArgs e)
        {
            string str;
            string str2;
            Form1 Form1 = new Form1();
            

            if (textBoxpersonatgenou.Text == null || textBoxintervalpunts.Text == null ||textBoxintervalpunts2.Text == null || pictureBox1.ImageLocation == null ||radioButtoncas== null && radioButtoncat==null && radioButtoneng==null)
            {

                MessageBox.Show("Omple totes les dades del formulari", "Aviso", MessageBoxButtons.OK, MessageBoxIcon.Error);

            }

            else if (radioButtoncat.Checked)
            {
                personaje1.nom = textBoxpersonatgenou.Text;
                personaje1.imagen = pictureBox1.ImageLocation;
                str = textBoxintervalpunts.Text;
                str2 = textBoxintervalpunts2.Text;

                personaje1.puntmin = Int32.Parse(str);
                personaje1.puntmax = Int32.Parse(str2);

                personajes.Add(personaje1);

                JArray jArraypersonatges = (JArray)JToken.FromObject(personajes);
                StreamWriter arxiuescriptura = File.CreateText(@"..\..\Catala_pers.json");
                JsonTextWriter escriurearxiu = new JsonTextWriter(arxiuescriptura);

                jArraypersonatges.WriteTo(escriurearxiu);

                escriurearxiu.Close();

                MessageBox.Show("El personatge s'ha guardat correctament", "Informacio", MessageBoxButtons.OK, MessageBoxIcon.Information);
                textBoxpersonatgenou.Clear();
                textBoxintervalpunts.Clear();
                textBoxintervalpunts2.Clear();
                pictureBox1.Image = null;
                radioButtoncat.Checked = false;
                radioButtoncas.Checked = false;
                radioButtoneng.Checked = false;
            }
            else if (radioButtoncas.Checked)
            {
                personaje1.nom = textBoxpersonatgenou.Text;
                personaje1.imagen = pictureBox1.ImageLocation;
                str = textBoxintervalpunts.Text;
                str2 = textBoxintervalpunts2.Text;

                personaje1.puntmin = Int32.Parse(str);
                personaje1.puntmax = Int32.Parse(str2);

                personajes.Add(personaje1);

                JArray jArraypersonatges = (JArray)JToken.FromObject(personajes);
       
                StreamWriter arxiuescriptura = File.CreateText(@"..\..\Castellano_pers.json");
                JsonTextWriter escriurearxiu = new JsonTextWriter(arxiuescriptura);

                jArraypersonatges.WriteTo(escriurearxiu);

                escriurearxiu.Close();

                MessageBox.Show("El personaje se ha guardado correctamente.", "Información", MessageBoxButtons.OK, MessageBoxIcon.Information);

                textBoxpersonatgenou.Clear();
                textBoxintervalpunts.Clear();
                textBoxintervalpunts2.Clear();
                pictureBox1.Image = null;
                radioButtoncat.Checked = false;
                radioButtoncas.Checked = false;
                radioButtoneng.Checked = false;
            }
            else if (radioButtoneng.Checked)
            {
                personaje1.nom = textBoxpersonatgenou.Text;
                personaje1.imagen = pictureBox1.ImageLocation;
                str = textBoxintervalpunts.Text;
                str2 = textBoxintervalpunts2.Text;

                personaje1.puntmin = Int32.Parse(str);
                personaje1.puntmax = Int32.Parse(str2);

                personajes.Add(personaje1);

                JArray jArraypersonatges = (JArray)JToken.FromObject(personajes);
                StreamWriter arxiuescriptura = File.CreateText(@"..\..\English_pers.json");
                JsonTextWriter escriurearxiu = new JsonTextWriter(arxiuescriptura);

                jArraypersonatges.WriteTo(escriurearxiu);

                escriurearxiu.Close();

                MessageBox.Show("The character has been saved correctly", "Information", MessageBoxButtons.OK, MessageBoxIcon.Information);

                textBoxpersonatgenou.Clear();
                textBoxintervalpunts.Clear();
                textBoxintervalpunts2.Clear();
                pictureBox1.Image = null;
                radioButtoncat.Checked = false;
                radioButtoncas.Checked = false;
                radioButtoneng.Checked = false;
            }
        }

       
    }
}


------------------------------------------AFEGIR PREGUNTES------------------------------

using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.IO;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;

namespace WindowsFormsApp1
{
    public partial class Form2 : Form
    {
        List<string> opcions = new List<string>();
        List<Preguntes1> preguntes = new List<Preguntes1>();

        public Form2()
        {
            InitializeComponent();
        }

        private void textBoxpreg_TextChanged(object sender, EventArgs e)
        {
           
        }

        private void button1_Click(object sender, EventArgs e)
        {
            this.Close();
        }

        private void buttoneliminaropcio_Click(object sender, EventArgs e)
        {
            string afegir = listBoxOpcions.SelectedItem.ToString();

           
            opcions.Remove(afegir);
            listBoxOpcions.DataSource = null;
            listBoxOpcions.DataSource = opcions;
            
        }

        private void buttonafegiropcio_Click(object sender, EventArgs e)
        {
            string afegir = textBoxopcio.Text;

            if(!opcions.Contains(afegir))
            {
                opcions.Add(afegir);
                listBoxOpcions.DataSource = null;
                listBoxOpcions.DataSource = opcions;
                textBoxopcio.Clear();
            }

        }

        private void buttonguardar3_Click(object sender, EventArgs e)
        {
            Preguntes1 preguntes2 = new Preguntes1();
            if (radioButtonCas == null && radioButtonCat == null && radioButtonEng == null || textBoxpreg == null || listBoxOpcions == null || textBoxresposta == null)
            {
                MessageBox.Show("Omple totes les dades del formulari", "Aviso", MessageBoxButtons.OK, MessageBoxIcon.Error);
            }


            else if (radioButtonCat.Checked)
            {


                if (radioButtonnivell1.Checked)
                {
                    preguntes2.nivell = 1;
                }
                else if (radioButtonnivell2.Checked)
                {
                    preguntes2.nivell = 2;
                }
                else if (radioButtonnivell3.Checked)
                {
                    preguntes2.nivell = 3;
                }
                preguntes2.pregunta = textBoxpreg.Text;
                preguntes2.opcio = opcions;
                preguntes2.resposta = textBoxresposta.Text;

                JArray jArraypreguntes = (JArray)JToken.FromObject(preguntes);
                StreamWriter arxiuescriptura = File.CreateText(@"..\..\Catala_preg.json");
                JsonTextWriter escriurearxiu = new JsonTextWriter(arxiuescriptura);

                jArraypreguntes.WriteTo(escriurearxiu);

                escriurearxiu.Close();
                MessageBox.Show("La pregunta s'ha guardat correctament", "Informacio", MessageBoxButtons.OK, MessageBoxIcon.Information);

            }
            else if (radioButtonCas.Checked)
            {
                if (radioButtonnivell1.Checked)
                {
                    preguntes2.nivell = 1;
                }
                else if (radioButtonnivell2.Checked)
                {
                    preguntes2.nivell = 2;
                }
                else if (radioButtonnivell3.Checked)
                {
                    preguntes2.nivell = 3;
                }
                preguntes2.pregunta = textBoxpreg.Text;
                preguntes2.opcio = opcions;
                preguntes2.resposta = textBoxresposta.Text;

                JArray jArraypreguntes = (JArray)JToken.FromObject(preguntes);
                StreamWriter arxiuescriptura = File.CreateText(@"..\..\Castellano_preg.json");
                JsonTextWriter escriurearxiu = new JsonTextWriter(arxiuescriptura);

                jArraypreguntes.WriteTo(escriurearxiu);

                escriurearxiu.Close();
                MessageBox.Show("La pregunta se ha guardado correctamente", "Informacio", MessageBoxButtons.OK, MessageBoxIcon.Information);

            }
            else if (radioButtonEng.Checked)
            {
                if (radioButtonnivell1.Checked)
                {
                    preguntes2.nivell = 1;
                }
                else if (radioButtonnivell2.Checked)
                {
                    preguntes2.nivell = 2;
                }
                else if (radioButtonnivell3.Checked)
                {
                    preguntes2.nivell = 3;
                }
                preguntes2.pregunta = textBoxpreg.Text;
                preguntes2.opcio = opcions;
                preguntes2.resposta = textBoxresposta.Text;

                JArray jArraypreguntes = (JArray)JToken.FromObject(preguntes);
                StreamWriter arxiuescriptura = File.CreateText(@"..\..\English_preg.json");
                JsonTextWriter escriurearxiu = new JsonTextWriter(arxiuescriptura);

                jArraypreguntes.WriteTo(escriurearxiu);

                escriurearxiu.Close();
                MessageBox.Show("The question has been saved correctly", "Information", MessageBoxButtons.OK, MessageBoxIcon.Information);

            }

        }
    }
}


--------------------------------MODIFICAR PERSONATGES----------------------------------------------

using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.IO;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;

namespace WindowsFormsApp1
{
    public partial class Modificar_personatges : Form
    {
        public Modificar_personatges()
        {
            InitializeComponent();
        }

        private void buttonsortir2_Click(object sender, EventArgs e)
        {
            this.Close();
        }

        private void buttonimatge_Click(object sender, EventArgs e)
        {
            OpenFileDialog Modificarimagen = new OpenFileDialog();
            Modificarimagen.Filter = "Arxiu de imatge|*.*";
            Modificarimagen.InitialDirectory = "\\ABP\\WindowsFormsApp1";

            if (Modificarimagen.ShowDialog()== DialogResult.OK)
            {
                File.Copy(Modificarimagen.FileName, @"..\..\..\imagen\" + Modificarimagen.SafeFileName);

                pictureBox1.ImageLocation = @"\..\..\..\imagen\" + Modificarimagen.SafeFileName;
                pictureBox1.SizeMode = PictureBoxSizeMode.StretchImage;
            }
        }
    }
}


--------------------------------------------------MODIFICAR PREGUNTES-------------------------------------

using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;

namespace WindowsFormsApp1
{
    public partial class Modificarpreguntes : Form
    {
        public Modificarpreguntes()
        {
            InitializeComponent();
        }

        private void buttonsortir4_Click(object sender, EventArgs e)
        {
            this.Close();
        }

        private void buttonafegir2_Click(object sender, EventArgs e)
        {
            string afegir = textBoxopcion2.Text;

            if (!listBoxopcions2.Items.Contains(afegir))
            {
                listBoxopcions2.Items.Add(afegir);
                textBoxopcion2.Clear();
            }
            else
            {
                MessageBox.Show("Aquesta opcio esta repetida.","INFORMACIO", MessageBoxButtons.OK, MessageBoxIcon.Error);
            }
        }

        private void buttoneliminar2_Click(object sender, EventArgs e)
        {
            listBoxopcions2.Items.Remove(listBoxopcions2.SelectedItem);  
        }
    }
}


---------------------------------Clase Personajes------------------------

using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace WindowsFormsApp1
{
    public class Personajes
    {
        public string nom { get; set; }
        public int puntmin { get; set; }
        public int puntmax { get; set; }
        public string imagen { get; set; }
    }


}

-----------------------------------Clase Preguntes1-------------------------------------

using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace WindowsFormsApp1
{
    public class Preguntes1
    {
        public int nivell { get; set; }
        public string pregunta { get; set; }
        public List<string> opcio { get; set; }
        public string resposta { get; set; }
    }
}


----------------------------------Catala_preg.json--------------------------------------------

  [
  {
    "nivell": 1,
    "pregunta": "Quin any va aterra el astronauta Neil Armstrong a la Lluna?",
    "opcio": ["1979", "1959", "1965" ],
    "resposta":  "1969"
  },

  {
    "nivell": 1,
    "pregunta": "Qué es la Lluna?",
    "opcio": [ "Una planeta", "Una constelació", "Una estrella" ],
    "resposta": "Un satélit"
  },

  {
    "nivell": 1,
    "pregunta": "Quants planetes hi han al sistema Solar?",
    "opcio": [ "7", "6", "10" ],
    "resposta": "8"
  },

    {
      "nivell": 1,
      "pregunta": "A quina galaxia es troba el sistema Solar",
      "opcio": [ "Andromeda", "Gran Núvol de Magalhães", "Triangle" ],
      "resposta": "Via Lactea"
    },

  {
    "nivell": 1,
    "pregunta": "Que es considera Plutó?",
    "opcio": [ "Satelit", "Planeta", "Planeta petit", "Meteorit" ],
    "resposta":  "Planeta petit"
  },


 

  {
    "nivell": 2,
    "pregunta": "Quanta distancia hi ha entre el Sol i la Terra?",
    "opcio": [ "154,6 milions de KM", "227,9 milions de KM", "A tres parades de bus" ],
    "resposta":  "149,6 milions de KM"
  },

    {
      "nivell": 2,
      "pregunta": "En quin planeta es trobava la raça Na'vi?(Avatar)",
      "opcio": [ "Plutó", "Kepler", "Ceres" ],
      "resposta": "Pandora"
    },

  {
    "nivell": 2,
    "pregunta": "Quin nom va rebre la missió del viatge a la Lluna?",
    "opcio": [ "Pioneer E", "Helios A", "SOHO" ],
    "resposta": "Apolo 11"
  },

  {
    "nivell": 2,
    "pregunta": "En quin planeta va neixer Anakin Skywalker?",
    "opcio": [ "Hoth", "Naboo", "Coruscant" ],
    "resposta": "Tatooine"
  },

  {
    "nivell": 2,
    "pregunta": "De que es va alimentar el astronauta de la pel·licula 'The Martian'?",
    "opcio": [ "Animals", "Insectes", "Fruita" ],
    "resposta":  "Verdures"
  },


 

  {
    "nivell": 3,
    "pregunta": "Quina es la primera pel·licula de ciencia ficció?",
    "opcio": [ "Viatge a Mart", "Viatge a l'espai", "Viatge al centre de la Terra" ],
    "resposta": "Viatge a la Lluna"
  },

  {
    "nivell": 3,
    "pregunta": "En quina pel·licula Anakin Skywalker es pasa al bàndol del Sith?",
    "opcio": [ "Star Wars: la amenaça fantasma",  "Star Wars: el atac dels clons", "Star Wars: el retorn del Jedi" ],
    "resposta": "Star Wars: la venjança dels Sith"
  },

  {
    "nivell": 3,
    "pregunta": "Quin any es va estrenar la pel·licula 2001: una odissea a l'espai?",
    "opcio": [ "1979", "2000", "1957"],
    "resposta": "1968"
  },

  {
    "nivell": 3,
    "pregunta": "Cada quants anys passa el cometa Halley?",
    "opcio": [ "80 anys", "63 anys", "71 anys" ],
    "resposta":  "75 anys"
  },

  {
    "nivell": 3,
    "pregunta": "Quanta gravetat hi ha a la Lluna?",
    "opcio": [ "9,86 metres per segon al cuadrat",  "5,28 metres per segon al cuadrat", "3,25 metres per segon al cuadrat" ],
    "resposta": "1,62 metres per segon al cuadrat"
  }
]

--------------------------------------Catala_pers.json----------------------------------------

[{"nom":"Darth Vader","puntmin":51,"puntmax":80,"imagen":"..\\..\\..\\imagen\\vader.jpg"},{"nom":"Mestre Yoda","puntmin":20,"puntmax":50,"imagen":"..\\..\\..\\imagen\\yoda.jpg"},{"nom":"Luke Skywalker","puntmin":100,"puntmax":150,"imagen":"..\\..\\..\\imagen\\Luke Skywalker2.jpg"}]


--------------------------------------Castella_preg.json--------------------------------------

[
  {
    "nivell": 1,
    "pregunta": "Que año aterrizo el astronauta Neil Armstrong a la Luna?",
    "opcio": [ "1979", "1959", "1965" ],
    "resposta": "1969"
  },

  {
    "nivell": 1,
    "pregunta": "Qué es la Luna?",
    "opcio": [ "Una planeta", "Una constelación", "Una estrella"],
    "resposta":  "Un satélite"
  },

  {
    "nivell": 1,
    "pregunta": "Cuantos planetas hay en el sistema Solar?",
    "opcio": [ "7","6", "10" ],
    "resposta": "8"
  },

  {
    "nivell": 1,
    "pregunta": "En que galaxia se encuentra el sistema Solar?",
    "opcio": [ "Andromeda", "Gran Nube de Magalhães", "Triangulo" ],
    "resposta": "Via Lactea"
  },

  {
    "nivell": 1,
    "pregunta": "Que se considera Plutón?",
    "opcio": [ "Satélite", "Planeta", "Meteorito" ],
    "resposta": "Planeta pequeño"
  },

 {
    "nivell": 2,
    "pregunta": "Cuanta distancia hay entre el Sol i la Tierra?",
  "opcio": [ "154,6 millones de KM", "227,9 millones de KM", "A tres paradas de bus" ],
  "resposta": "149,6 millones de KM"
  },

  {
    "nivell": 2,
    "pregunta": "En que planeta se encuentra la raza Na'vi?(Avatar)",
    "opcio": [ "Plutón", "Kepler", "Ceres" ],
    "resposta": "Pandora"
  },

  {
    "nivell": 2,
    "pregunta": "Que nombre recibió la missión del viage a la Luna?",
    "opcio": ["Pioneer E", "Helios A", "SOHO" ],
    "resposta": "Apolo 11 "
  },

  {
    "nivell": 2,
    "pregunta": "En que planeta nacio Anakin Skywalker?",
    "opcio": [ "Hoth", "Naboo","Coruscant" ],
    "resposta": "Tatooine"
  },

  {
    "nivell": 2,
    "pregunta": "De que se alimento el astronauta de la pelicula 'The Martian'?",
    "opcio": [ "Animales", "Insectos", "Fruta"],
    "resposta": "Verduras"
  },

  {
    "nivell": 3,
    "pregunta": "Cual es la primera pelicula de ciencia ficción?",
    "opcio": ["Viaje a Marte", "Viaje al espacio", "Viaje al centro de la Tierra" ],
    "resposta": "Viaje a la Luna"
  },

  {
    "nivell": 3,
    "pregunta": "En que pelicula Anakin Skywalker se pasa al bando de los Sith?",
    "opcio": [ "Star Wars: la amenaza fantasma", "Star Wars: el ataque de los clones", "Star Wars: el retorno Jedi" ],
    "resposta": "Star Wars: la venganza de los Sith"
  },

  {
    "nivell": 3,
    "pregunta": "Que año se estreno la pelicula 2001: una odissea en el espacio?",
    "opcio": [ "1979", "2000", "1957"],
    "resposta": "1968"
  },

  {
    "nivell": 3,
    "pregunta": "Cada cuantos años pasa el cometa Halley?",
    "opcio": [ "80 años", "63 años","71 años" ],
    "resposta": "75 años"
  },

  {
    "nivell": 3,
    "pregunta": "Cuanta gravedad hay en la Luna?",
    "opcio": [ "9,86 metros por segundo al cuadrado", "5,28 metros por segundo al cuadrado", "3,25 metros por segundo al cuadrado" ],
    "resposta": "1,62 metros por segundo al cuadrado"
  }
]


---------------------------------------Castellano_pers.json------------------------------------

[{"nom":"fdgdfg","puntmin":33,"puntmax":66,"imagen":"..\\..\\..\\imagen\\pepe2.jpg"}]


---------------------------------------English_preg.json----------------------------------------

[
  {
    "nivell": 1,
    "pregunta": "What year did Neil Armstrong land on the moon?",
    "opcio": [ "1979", "1959", "1965" ],
    "resposta": "1969"
  },

  {
    "nivell": 1,
    "pregunta": "What is the moon?",
    "opcio": [ "A planet", "A constellation", "Una estrella" ],
    "resposta": "A satellite"
  },

  {
    "nivell": 1,
    "pregunta": "How many planets are in the solar system?",
    "opcio": [ "7", "6", "10" ],
    "resposta": "8"
  },

  {
    "nivell": 1,
    "pregunta": "In which galaxy is the solar system?",
    "opcio": [ "Andromeda", "Great Cloud of Magalhães", "Triangle" ],
    "resposta": "Milky Way"
  },

  {
    "nivell": 1,
    "pregunta": "What is considered Pluton?",
    "opcio": [ "A satellite", "A planet", "Meteorite" ],
    "resposta": "Small planet"
  },

  {
    "nivell": 2,
    "pregunta": "How much distance is there between the sun and the earth?",
    "opcio": [ "154,6 millions KM", "227,9 millions KM", "Three bus stops" ],
    "resposta": "149,6 millions KM"
  },

  {
    "nivell": 2,
    "pregunta": "On which planet is the Na'vi race? (Avatar)",
    "opcio": [ "Pluton", "Kepler", "Ceres" ],
    "resposta": "Pandora"
  },

  {
    "nivell": 2,
    "pregunta": "What name did the mission of the journey to the Moon receive?",
    "opcio": [ "Pioneer E", "Helios A", "SOHO" ],
    "resposta": "Apolo 11 "
  },

  {
    "nivell": 2,
    "pregunta": "On which planet Anakin Skywalker was born?",
    "opcio": [ "Hoth", "Naboo", "Coruscant" ],
    "resposta": "Tatooine"
  },

  {
    "nivell": 2,
    "pregunta": "What did the astronaut feed from the movie 'The Martian'?",
    "opcio": [ "Animals", "Insects", "Fruit" ],
    "resposta": "Vegetables"
  },

  {
    "nivell": 3,
    "pregunta": "What is the first science fiction movie?",
    "opcio": [ "Trip to Mars", "Trip to space", "Journey to the Center of the Earth" ],
    "resposta": "Journey to the Moon"
  },

  {
    "nivell": 3,
    "pregunta": "In which movie Anakin Skywalker is passed to the side of the Sith?",
    "opcio": [ "Star Wars: The Phantom Menace", "Star Wars: Attack of the clones", "Star Wars: the Jedi return" ],
    "resposta": "Star Wars: the revenge of the Sith"
  },

  {
    "nivell": 3,
    "pregunta": "What year was the 2001 movie premiere: an odissea in space?",
    "opcio": [ "1979", "2000", "1957" ],
    "resposta": "1968"
  },

  {
    "nivell": 3,
    "pregunta": "How often does Halley's comet pass?",
    "opcio": [ "80 años", "63 años", "71 años" ],
    "resposta": "75 años"
  },

  {
    "nivell": 3,
    "pregunta": "How much gravity is there on the moon?",
    "opcio": [ "9,86 meters per second squared", "5,28 meters per second squared", "3,25 meters per second squared" ],
    "resposta": "1,62 meters per second squared"
  }
]


-------------------------------------------English_pers.json--------------------------------------

[{"nom":"Master Yoda","puntmin":20,"puntmax":50,"imagen":"..\\..\\..\\imagen\\yoda2.jpeg"},{"nom":"Luke Skywalker","puntmin":100,"puntmax":150,"imagen":"..\\..\\..\\imagen\\Luke Skywalker.jpg"}]


