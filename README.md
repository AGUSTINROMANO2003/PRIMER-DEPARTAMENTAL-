# PRIMER-DEPARTAMENTAL 
PRACTICA 1-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Hola Mundo Flutter',
      home: Scaffold(
        appBar: AppBar(
          title: const Text('Hola Mundo'),
        ),
        body: const Center(
          child: Text(
            '¡Hola Mundo!',
            style: TextStyle(fontSize: 24),
          ),
        ),
      ),
    );
  }
}

PRACTICA 2----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
// Importa el paquete principal de Flutter con Material Design
import 'package:flutter/material.dart';

// Función principal: punto de entrada de la app
void main() {
  runApp(const MaterialApp(
    home: HolaMundo(), // Widget principal
  ));
}

// Widget con estado (StatefulWidget)
class HolaMundo extends StatefulWidget {
  const HolaMundo({super.key});

  @override
  State<HolaMundo> createState() => _HolaMundoState();
}

// Estado del widget HolaMundo
class _HolaMundoState extends State<HolaMundo> {

  // Lista donde se almacenan los mensajes "Hola mundo"
  List<String> mensajes = [];

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      // Barra superior de la aplicación
      appBar: AppBar(
        title: const Text('Hola Mundo'),
      ),

      // Cuerpo de la pantalla
      body: Column(
        children: [

          // Botón para agregar un nuevo mensaje
          ElevatedButton(
            onPressed: () {
              setState(() {
                // Agrega un nuevo "Hola mundo" a la lista
                mensajes.add('Hola mundo');
              });
            },
            child: const Text('Agregar'),
          ),

          // Ocupa el espacio restante de la pantalla
          Expanded(
            child: ListView(
              children: mensajes
                  // Convierte cada mensaje en un widget Text
                  .map((m) => Text(m))
                  .toList(),
            ),
          ),
        ],
      ),
    );
  }
}
}
PRACTICA 3 *-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------





PRACTICA 4-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
import 'package:flutter/material.dart';

void main() {
  runApp(const RegistroApp());
}

// Widget raíz de la aplicación
class RegistroApp extends StatelessWidget {
  const RegistroApp({super.key});

  @override
  Widget build(BuildContext context) {
    return const MaterialApp(
      debugShowCheckedModeBanner: false,
      home: RegistroScreen(),
    );
  }
}

// Pantalla principal de registro (con estado)
class RegistroScreen extends StatefulWidget {
  const RegistroScreen({super.key});

  @override
  State<RegistroScreen> createState() => _RegistroScreenState();
}

class _RegistroScreenState extends State<RegistroScreen> {
  // Clave global para manejar el estado del formulario
  final GlobalKey<FormState> _formKey = GlobalKey<FormState>();

  // Controladores para obtener el texto de los campos
  final TextEditingController _nombreController = TextEditingController();
  final TextEditingController _emailController = TextEditingController();
  final TextEditingController _passwordController = TextEditingController();
  final TextEditingController _confirmPasswordController = TextEditingController();

  // Nodos de foco para controlar el avance entre campos
  final FocusNode _emailFocus = FocusNode();
  final FocusNode _passwordFocus = FocusNode();
  final FocusNode _confirmPasswordFocus = FocusNode();

  // Control de UI y validación
  bool _ocultarPassword = true;
  bool _aceptaTerminos = false;
  AutovalidateMode _autoValidate = AutovalidateMode.disabled;

  @override
  void dispose() {
    // Liberar recursos
    _nombreController.dispose();
    _emailController.dispose();
    _passwordController.dispose();
    _confirmPasswordController.dispose();
    _emailFocus.dispose();
    _passwordFocus.dispose();
    _confirmPasswordFocus.dispose();
    super.dispose();
  }

  // Método para limpiar el formulario
  void _limpiarFormulario() {
    _formKey.currentState?.reset();
    _nombreController.clear();
    _emailController.clear();
    _passwordController.clear();
    _confirmPasswordController.clear();
    setState(() {
      _aceptaTerminos = false;
      _ocultarPassword = true;
      _autoValidate = AutovalidateMode.disabled;
    });
  }

  // Método para enviar el formulario
  void _enviarFormulario() {
    setState(() {
      _autoValidate = AutovalidateMode.onUserInteraction;
    });

    if (_formKey.currentState!.validate() && _aceptaTerminos) {
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(
          content: Text(
            'Registrado: ${_nombreController.text} (${_emailController.text})',
          ),
        ),
      );
    } else if (!_aceptaTerminos) {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(
          content: Text('Debes aceptar los términos y condiciones'),
        ),
      );
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Registro'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16),
        child: Form(
          key: _formKey,
          autovalidateMode: _autoValidate,
          child: SingleChildScrollView(
            child: Column(
              children: [
                // Campo Nombre
                TextFormField(
                  controller: _nombreController,
                  textInputAction: TextInputAction.next,
                  decoration: const InputDecoration(
                    labelText: 'Nombre',
                    prefixIcon: Icon(Icons.person),
                    border: OutlineInputBorder(),
                  ),
                  onFieldSubmitted: (_) =>
                      FocusScope.of(context).requestFocus(_emailFocus),
                  validator: (value) {
                    if (value == null || value.trim().isEmpty) {
                      return 'El nombre es obligatorio';
                    }
                    if (value.trim().length < 3) {
                      return 'Debe tener al menos 3 caracteres';
                    }
                    return null;
                  },
                ),

                const SizedBox(height: 12),

                // Campo Email
                TextFormField(
                  controller: _emailController,
                  focusNode: _emailFocus,
                  keyboardType: TextInputType.emailAddress,
                  textInputAction: TextInputAction.next,
                  decoration: const InputDecoration(
                    labelText: 'Email',
                    prefixIcon: Icon(Icons.email),
                    border: OutlineInputBorder(),
                  ),
                  onFieldSubmitted: (_) =>
                      FocusScope.of(context).requestFocus(_passwordFocus),
                  validator: (value) {
                    if (value == null || value.trim().isEmpty) {
                      return 'El email es obligatorio';
                    }
                    final emailRegex =
                        RegExp(r'^[\w-\.]+@([\w-]+\.)+[\w-]{2,4}$');
                    if (!emailRegex.hasMatch(value.trim())) {
                      return 'Formato de email inválido';
                    }
                    return null;
                  },
                ),

                const SizedBox(height: 12),

                // Campo Contraseña
                TextFormField(
                  controller: _passwordController,
                  focusNode: _passwordFocus,
                  obscureText: _ocultarPassword,
                  textInputAction: TextInputAction.next,
                  decoration: InputDecoration(
                    labelText: 'Contraseña',
                    prefixIcon: const Icon(Icons.lock),
                    border: const OutlineInputBorder(),
                    suffixIcon: IconButton(
                      icon: Icon(
                        _ocultarPassword
                            ? Icons.visibility
                            : Icons.visibility_off,
                      ),
                      onPressed: () {
                        setState(() {
                          _ocultarPassword = !_ocultarPassword;
                        });
                      },
                    ),
                  ),
                  onFieldSubmitted: (_) => FocusScope.of(context)
                      .requestFocus(_confirmPasswordFocus),
                  validator: (value) {
                    if (value == null || value.isEmpty) {
                      return 'La contraseña es obligatoria';
                    }
                    if (value.length < 6) {
                      return 'Mínimo 6 caracteres';
                    }
                    return null;
                  },
                ),

                const SizedBox(height: 12),

                // Campo Confirmar Contraseña
                TextFormField(
                  controller: _confirmPasswordController,
                  focusNode: _confirmPasswordFocus,
                  obscureText: _ocultarPassword,
                  textInputAction: TextInputAction.done,
                  decoration: const InputDecoration(
                    labelText: 'Confirmar contraseña',
                    prefixIcon: Icon(Icons.lock_outline),
                    border: OutlineInputBorder(),
                  ),
                  validator: (value) {
                    if (value != _passwordController.text) {
                      return 'Las contraseñas no coinciden';
                    }
                    return null;
                  },
                ),

                const SizedBox(height: 12),

                // Checkbox de términos
                CheckboxListTile(
                  value: _aceptaTerminos,
                  onChanged: (value) {
                    setState(() {
                      _aceptaTerminos = value ?? false;
                    });
                  },
                  title: const Text('Acepto los términos y condiciones'),
                  controlAffinity: ListTileControlAffinity.leading,
                ),

                const SizedBox(height: 16),

                // Botón Enviar
                ElevatedButton(
                  onPressed: _enviarFormulario,
                  child: const Text('Enviar'),
                ),

                const SizedBox(height: 8),

                // Botón Limpiar
                OutlinedButton(
                  onPressed: _limpiarFormulario,
                  child: const Text('Limpiar'),
                ),
              ],
            ),
          ),
        ),
      ),
    );
  }
}

PRACTICA 5 ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
import 'dart:math';
import 'package:flutter/material.dart';

void main() {
  runApp(const DrawerApp());
}

// Aplicación principal
class DrawerApp extends StatelessWidget {
  const DrawerApp({super.key});

  @override
  Widget build(BuildContext context) {
    return const MaterialApp(
      debugShowCheckedModeBanner: false,
      home: HomeScreen(),
    );
  }
}

// Pantalla base con Drawer
class HomeScreen extends StatefulWidget {
  const HomeScreen({super.key});

  @override
  State<HomeScreen> createState() => _HomeScreenState();
}

class _HomeScreenState extends State<HomeScreen> {
  // Controla qué pantalla se muestra
  Widget _pantallaActual = const PracticaScreen(titulo: 'Práctica 1');

  // Método para cambiar pantalla desde el Drawer
  void _navegar(Widget pantalla) {
    setState(() {
      _pantallaActual = pantalla;
    });
    Navigator.pop(context); // Cierra el Drawer
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Menú de Prácticas'),
      ),
      drawer: Drawer(
        child: ListView(
          children: [
            const DrawerHeader(
              decoration: BoxDecoration(color: Colors.blue),
              child: Center(
                child: Text(
                  'Prácticas Flutter',
                  style: TextStyle(color: Colors.white, fontSize: 22),
                ),
              ),
            ),
            ListTile(
              leading: const Icon(Icons.book),
              title: const Text('Práctica 1'),
              onTap: () => _navegar(const PracticaScreen(titulo: 'Práctica 1')),
            ),
            ListTile(
              leading: const Icon(Icons.book),
              title: const Text('Práctica 2'),
              onTap: () => _navegar(const PracticaScreen(titulo: 'Práctica 2')),
            ),
            ListTile(
              leading: const Icon(Icons.book),
              title: const Text('Práctica 3'),
              onTap: () => _navegar(const PracticaScreen(titulo: 'Práctica 3')),
            ),
            ListTile(
              leading: const Icon(Icons.book),
              title: const Text('Práctica 4'),
              onTap: () => _navegar(const PracticaScreen(titulo: 'Práctica 4')),
            ),
            const Divider(),
            ListTile(
              leading: const Icon(Icons.sports_esports),
              title: const Text('Juego: Piedra, Papel o Tijera'),
              onTap: () => _navegar(const JuegoRPS()),
            ),
          ],
        ),
      ),
      body: _pantallaActual,
    );
  }
}

// Pantallas placeholder para prácticas anteriores
class PracticaScreen extends StatelessWidget {
  final String titulo;

  const PracticaScreen({super.key, required this.titulo});

  @override
  Widget build(BuildContext context) {
    return Center(
      child: Text(
        titulo,
        style: const TextStyle(fontSize: 28),
      ),
    );
  }
}

// Juego Piedra, Papel o Tijera
class JuegoRPS extends StatefulWidget {
  const JuegoRPS({super.key});

  @override
  State<JuegoRPS> createState() => _JuegoRPSState();
}

class _JuegoRPSState extends State<JuegoRPS> {
  final List<String> _opciones = ['Piedra', 'Papel', 'Tijera'];
  final Random _random = Random();

  String _eleccionUsuario = '';
  String _eleccionApp = '';
  String _resultado = '';

  int _puntosUsuario = 0;
  int _puntosApp = 0;

  // Lógica del juego
  void _jugar(String eleccion) {
    String eleccionApp = _opciones[_random.nextInt(3)];
    String resultado;

    if (eleccion == eleccionApp) {
      resultado = 'Empate';
    } else if (
        (eleccion == 'Piedra' && eleccionApp == 'Tijera') ||
        (eleccion == 'Papel' && eleccionApp == 'Piedra') ||
        (eleccion == 'Tijera' && eleccionApp == 'Papel')) {
      resultado = 'Ganaste';
      _puntosUsuario++;
    } else {
      resultado = 'Perdiste';
      _puntosApp++;
    }

    setState(() {
      _eleccionUsuario = eleccion;
      _eleccionApp = eleccionApp;
      _resultado = resultado;
    });
  }

  // Reinicia el marcador
  void _reiniciar() {
    setState(() {
      _puntosUsuario = 0;
      _puntosApp = 0;
      _resultado = '';
      _eleccionUsuario = '';
      _eleccionApp = '';
    });
  }

  @override
  Widget build(BuildContext context) {
    return Padding(
      padding: const EdgeInsets.all(16),
      child: Column(
        mainAxisAlignment: MainAxisAlignment.center,
        children: [
          const Text(
            'Piedra, Papel o Tijera',
            style: TextStyle(fontSize: 24, fontWeight: FontWeight.bold),
          ),
          const SizedBox(height: 16),

          Text('Usuario: $_puntosUsuario  |  App: $_puntosApp',
              style: const TextStyle(fontSize: 18)),

          const SizedBox(height: 20),

          Row(
            mainAxisAlignment: MainAxisAlignment.spaceEvenly,
            children: _opciones.map((opcion) {
              return ElevatedButton(
                onPressed: () => _jugar(opcion),
                child: Text(opcion),
              );
            }).toList(),
          ),

          const SizedBox(height: 20),

          Text(
            _resultado,
            style: const TextStyle(fontSize: 20, fontWeight: FontWeight.bold),
          ),

          const SizedBox(height: 10),

          Text('Tu elección: $_eleccionUsuario'),
          Text('Elección de la app: $_eleccionApp'),

          const SizedBox(height: 20),

          OutlinedButton(
            onPressed: _reiniciar,
            child: const Text('Reiniciar marcador'),
          ),
        ],
      ),
    );
  }
}

PROYECTO 1ER DEPARTAMENTAL---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
import 'package:flutter/material.dart'; import 'package:provider/provider.dart'; import 'dart:math';

// ------------------- INICIO: Lógica Principal y de Tema -------------------


// Punto de entrada de la aplicación.
// Se configura el `ChangeNotifierProvider` para manejar el estado del tema. void main() {
runApp(
ChangeNotifierProvider(
create: (_) => ThemeProvider(), child: const PortfolioApp(),
),
);
}


// Widget raíz de la aplicación.
class PortfolioApp extends StatelessWidget { const PortfolioApp({super.key});

@override
Widget build(BuildContext context) {
// `Consumer` escucha los cambios en `ThemeProvider` para reconstruir la UI cuando cambia el tema.
 
return Consumer<ThemeProvider>(
builder: (context, themeProvider, child) { return MaterialApp(
title: 'Portafolio App',
debugShowCheckedModeBanner: false,
// Tema para el modo claro. theme: ThemeData(
useMaterial3: true,
brightness: Brightness.light,
colorScheme: ColorScheme.fromSeed(seedColor: Colors.deepPurple),
),
// Tema para el modo oscuro. darkTheme: ThemeData(
useMaterial3: true,
brightness: Brightness.dark,
colorScheme: ColorScheme.fromSeed( seedColor: Colors.deepPurple, brightness: Brightness.dark,
),
),
// El `themeMode` actual se obtiene del `ThemeProvider`. themeMode: themeProvider.themeMode,
// Ruta inicial de la aplicación. initialRoute: '/',
// Definición de las rutas nombradas para la navegación. routes: {
 
'/': (context) => const HubScreen(),
'/practices': (context) => const PracticesScreen(), '/project': (context) => const ProjectKitScreen(), '/settings': (context) => const SettingsScreen(),
},
);
},
);
}
}


// Clase que maneja el estado del tema (claro/oscuro). class ThemeProvider extends ChangeNotifier {
ThemeMode _themeMode = ThemeMode.system; ThemeMode get themeMode => _themeMode;

void toggleTheme(bool isDarkMode) {
_themeMode = isDarkMode ? ThemeMode.dark : ThemeMode.light;
// Notifica a los `listeners` (los `Consumer`) que el estado ha cambiado. notifyListeners();
}
}


// ------------------- INICIO: Widgets Reutilizables -------------------


// Menú de navegación lateral (Drawer) que se usa en todas las pantallas principales.
 
class AppDrawer extends StatelessWidget { const AppDrawer({super.key});

@override
Widget build(BuildContext context) { return Drawer(
child: ListView(
padding: EdgeInsets.zero, children: <Widget>[
const DrawerHeader(
decoration: BoxDecoration(color: Colors.deepPurple),
child: Text('Menú Principal', style: TextStyle(color: Colors.white, fontSize: 24)),
),
ListTile(
leading: const Icon(Icons.home), title: const Text('Inicio (Hub)'),
onTap: () => Navigator.pushReplacementNamed(context, '/'),
),
ListTile(
leading: const Icon(Icons.list_alt), title: const Text('Índice de Prácticas'),
onTap: () => Navigator.pushReplacementNamed(context, '/practices'),
),
ListTile(
leading: const Icon(Icons.build_circle), title: const Text('Proyecto: Kit Offline'),
 
onTap: () => Navigator.pushReplacementNamed(context, '/project'),
),
const Divider(), ListTile(
leading: const Icon(Icons.settings), title: const Text('Ajustes'),
onTap: () => Navigator.pushNamed(context, '/settings'),
),
],
),
);
}
}


// ------------------- INICIO: Pantallas Principales -------------------


// Pantalla principal que funciona como un panel de control. class HubScreen extends StatelessWidget {
const HubScreen({super.key});


@override
Widget build(BuildContext context) { return Scaffold(
appBar: AppBar(
title: const Text('Hub Principal'),
backgroundColor: Theme.of(context).colorScheme.inversePrimary,
 
),
drawer: const AppDrawer(), body: Padding(
padding: const EdgeInsets.all(16.0), child: GridView.count(
crossAxisCount: 2,
crossAxisSpacing: 16,
mainAxisSpacing: 16, children: <Widget>[
_buildHubCard(context, icon: Icons.list_alt, label: 'Prácticas', route: '/practices'),
_buildHubCard(context, icon: Icons.build, label: 'Proyecto Kit Offline', route: '/project'),
_buildHubCard(context, icon: Icons.settings, label: 'Ajustes', route: '/settings'),
],
),
),
);
}


// Widget helper para construir las tarjetas del Hub.
Widget _buildHubCard(BuildContext context, {required IconData icon, required String label, required String route}) {
return Card( elevation: 4,
child: InkWell(
onTap: () => Navigator.pushNamed(context, route), borderRadius: BorderRadius.circular(12),
 
child: Column(
mainAxisAlignment: MainAxisAlignment.center, children: <Widget>[
Icon(icon, size: 50, color: Theme.of(context).colorScheme.primary), const SizedBox(height: 10),
Text(label, textAlign: TextAlign.center, style: Theme.of(context).textTheme.titleMedium),
],
),
),
);
}
}


// Pantalla que muestra una lista de las prácticas. class PracticesScreen extends StatelessWidget { const PracticesScreen({super.key});

@override
Widget build(BuildContext context) {
final practices = List.generate(8, (i) => 'Práctica ${i + 1}'); return Scaffold(
appBar: AppBar(
title: const Text('Índice de Prácticas'),
backgroundColor: Theme.of(context).colorScheme.inversePrimary,
),
 
drawer: const AppDrawer(), body: ListView.builder(
itemCount: practices.length, itemBuilder: (context, index) { return Card(
margin: const EdgeInsets.symmetric(horizontal: 16, vertical: 8), child: ListTile(
leading: CircleAvatar(child: Text('${index + 1}')), title: Text(practices[index]),
trailing: const Icon(Icons.arrow_forward_ios), onTap: () {
// Muestra un SnackBar como placeholder de la navegación. ScaffoldMessenger.of(context)
..hideCurrentSnackBar()
..showSnackBar(SnackBar(content: Text('Navegando a ${practices[index]}...')));
},
),
);
},
),
);
}
}


// Pantalla de ajustes de la aplicación.
class SettingsScreen extends StatelessWidget {
 
const SettingsScreen({super.key});


@override
Widget build(BuildContext context) {
final themeProvider = context.watch<ThemeProvider>();
final isDarkMode = themeProvider.themeMode == ThemeMode.dark; return Scaffold(
appBar: AppBar(
title: const Text('Ajustes'),
backgroundColor: Theme.of(context).colorScheme.inversePrimary,
),
drawer: const AppDrawer(), body: ListView(
children: [ SwitchListTile(
title: const Text('Tema Oscuro'), value: isDarkMode,
onChanged: (value) => context.read<ThemeProvider>().toggleTheme(value), secondary: Icon(isDarkMode ? Icons.dark_mode : Icons.light_mode),
),
const Divider(), ListTile(
leading: const Icon(Icons.info_outline), title: const Text('Acerca de'),
onTap: () => showAboutDialog( context: context,
 
applicationName: 'Portafolio App', applicationVersion: '1.0.0',
applicationLegalese: '© 2024 [Tu Nombre]',
),
),
],
),
);
}
}


// ------------------- INICIO: Pantalla y Módulos del Proyecto "Kit Offline" -------------------


// Pantalla principal del proyecto que contiene los 4 módulos en pestañas. class ProjectKitScreen extends StatelessWidget {
const ProjectKitScreen({super.key}); @override
Widget build(BuildContext context) { return DefaultTabController( length: 4,
child: Scaffold( appBar: AppBar(
title: const Text('Proyecto: Kit Offline'),
backgroundColor: Theme.of(context).colorScheme.inversePrimary, bottom: const TabBar(
isScrollable: true,
 
tabs: [
Tab(icon: Icon(Icons.note), text: 'Notas'), Tab(icon: Icon(Icons.monitor_weight), text: 'IMC'),
Tab(icon: Icon(Icons.photo_library), text: 'Galería'), Tab(icon: Icon(Icons.casino), text: 'Juego'),
],
),
),
drawer: const AppDrawer(), body: const TabBarView( children: [
NotesModule(),
BmiCalculatorModule(), LocalGalleryModule(),
OddEvenGameModule(),
],
),
),
);
}
}


// Módulo 1: Notas Rápidas.
class NotesModule extends StatefulWidget { const NotesModule({super.key});
@override
 
State<NotesModule> createState() => _NotesModuleState();
}


class _NotesModuleState extends State<NotesModule> { final List<String> _notes = [];
final _noteController = TextEditingController();


void _addNote() {
if (_noteController.text.isNotEmpty) {
setState(() => _notes.add(_noteController.text));
_noteController.clear();
ScaffoldMessenger.of(context)..hideCurrentSnackBar()..showSnackBar(const SnackBar(content: Text('Nota agregada.')));
}
}


@override
Widget build(BuildContext context) { return Scaffold(
body: _notes.isEmpty
? const Center(child: Text('No hay notas aún. ¡Añade una!'))
: ListView.builder( itemCount: _notes.length,
itemBuilder: (context, index) => Card(
margin: const EdgeInsets.symmetric(horizontal: 8, vertical: 4), child: ListTile(title: Text(_notes[index])),
 
),
),
floatingActionButton: FloatingActionButton( onPressed: _addNoteWithDialog,
tooltip: 'Agregar Nota',
child: const Icon(Icons.add),
),
);
}


void _addNoteWithDialog() { showDialog(
context: context, builder: (context) {
return AlertDialog(
title: const Text('Nueva Nota'), content: TextField(
controller: _noteController, autofocus: true,
decoration: const InputDecoration(hintText: 'Escribe tu nota aquí'),
),
actions: [
TextButton(onPressed: () => Navigator.pop(context), child: const Text('Cancelar')), ElevatedButton(
onPressed: () {
_addNote();
 
Navigator.pop(context);
},
child: const Text('Guardar'),
),
],
);
},
);
}
}


// Módulo 2: Calculadora de IMC.
class BmiCalculatorModule extends StatefulWidget { const BmiCalculatorModule({super.key});
@override
State<BmiCalculatorModule> createState() => _BmiCalculatorModuleState();
}


class _BmiCalculatorModuleState extends State<BmiCalculatorModule> { final _formKey = GlobalKey<FormState>();
final _heightController = TextEditingController(); final _weightController = TextEditingController();

void _calculateBmi() {
if (_formKey.currentState!.validate()) {
final double height = double.parse(_heightController.text);
 
final double weight = double.parse(_weightController.text); final double bmi = weight / (height * height);
String category;
if (bmi < 18.5) category = 'Bajo peso'; else if (bmi < 25) category = 'Normal';
else if (bmi < 30) category = 'Sobrepeso'; else category = 'Obesidad';
ScaffoldMessenger.of(context)..hideCurrentSnackBar()..showSnackBar(SnackBar(conte nt: Text('Tu IMC es ${bmi.toStringAsFixed(2)} ($category)')));
}
}


void _clearForm() {
_formKey.currentState!.reset();
_heightController.clear();
_weightController.clear();
FocusScope.of(context).unfocus(); // Oculta el teclado.
}


@override
Widget build(BuildContext context) { return SingleChildScrollView(
padding: const EdgeInsets.all(16.0), child: Form(
key: _formKey, child: Column(
 
crossAxisAlignment: CrossAxisAlignment.stretch, children: [
TextFormField(
controller: _heightController,
decoration: const InputDecoration(labelText: 'Estatura (m)', border: OutlineInputBorder()),
keyboardType: const TextInputType.numberWithOptions(decimal: true),
validator: (v) => (v == null || v.isEmpty || double.tryParse(v) == null || double.parse(v)
<= 0) ? 'Ingresa un número positivo válido.' : null,
),
const SizedBox(height: 16), TextFormField(
controller: _weightController,
decoration: const InputDecoration(labelText: 'Peso (kg)', border: OutlineInputBorder()),
keyboardType: const TextInputType.numberWithOptions(decimal: true),
validator: (v) => (v == null || v.isEmpty || double.tryParse(v) == null || double.parse(v)
<= 0) ? 'Ingresa un número positivo válido.' : null,
),
const SizedBox(height: 24),
ElevatedButton(onPressed: _calculateBmi, child: const Text('Calcular IMC')), OutlinedButton(onPressed: _clearForm, child: const Text('Limpiar')),
],
),
),
);
}
 
}


// Módulo 3: Galería de Imágenes Locales.
class LocalGalleryModule extends StatelessWidget { const LocalGalleryModule({super.key});
@override
Widget build(BuildContext context) {
// Asegúrate de tener estas imágenes en `assets/images/` y declaradas en pubspec.yaml
final images = List.generate(6, (i) => 'assets/images/gallery_${i + 1}.png'); return Padding(
padding: const EdgeInsets.all(8.0), child: GridView.builder(
gridDelegate: const SliverGridDelegateWithFixedCrossAxisCount(crossAxisCount: 2, crossAxisSpacing: 8, mainAxisSpacing: 8),
itemCount: images.length, itemBuilder: (context, index) { final imagePath = images[index]; return GestureDetector(
onTap: () => showDialog( context: context,
builder: (context) => AlertDialog( title: Text('Imagen ${index + 1}'), content: Image.asset(imagePath),
actions: [TextButton(onPressed: () => Navigator.of(context).pop(), child: const Text('Cerrar'))],
),
 
),
child: Card(
clipBehavior: Clip.antiAlias,
child: Image.asset(imagePath, fit: BoxFit.cover, errorBuilder: (c, e, s) => const Icon(Icons.broken_image, size: 40, color: Colors.grey)),
),
);
},
),
);
}
}


// Módulo 4: Juego de Par o Impar.
class OddEvenGameModule extends StatefulWidget { const OddEvenGameModule({super.key});
@override
State<OddEvenGameModule> createState() => _OddEvenGameModuleState();
}


class _OddEvenGameModuleState extends State<OddEvenGameModule> { int _userScore = 0;
int _cpuScore = 0; String? _userChoice;

void _play(int userNumber) {
 
if (_userChoice == null) {
ScaffoldMessenger.of(context)..hideCurrentSnackBar()..showSnackBar(const SnackBar(content: Text('Por favor, elige Par o Impar primero.')));
return;
}
final cpuNumber = Random().nextInt(6); final total = userNumber + cpuNumber; final isTotalEven = total % 2 == 0;
bool userWon = (isTotalEven CC _userChoice == 'Par') || (!isTotalEven CC _userChoice == 'Impar');


if (userWon) setState(() => _userScore++); else setState(() => _cpuScore++);

ScaffoldMessenger.of(context)..hideCurrentSnackBar()..showSnackBar(SnackBar(conte nt: Text('${userWon ? '¡Ganaste!' : 'Perdiste.'} (Tu: $userNumber + CPU: $cpuNumber =
$total)')));
}


void _resetGame() => setState(() {
_userScore = 0;
_cpuScore = 0;
_userChoice = null;
});


@override
Widget build(BuildContext context) {
 
return SingleChildScrollView(
padding: const EdgeInsets.all(16.0), child: Column(
children: [ Card(
child: Padding(
padding: const EdgeInsets.all(16.0), child: Row(
mainAxisAlignment: MainAxisAlignment.spaceAround, children: [
Text('Usuario: $_userScore', style: Theme.of(context).textTheme.headlineSmall), Text('CPU: $_cpuScore', style: Theme.of(context).textTheme.headlineSmall),
],
),
),
),
const SizedBox(height: 24),
const Text('1. Elige Par o Impar:', style: TextStyle(fontSize: 16)), SegmentedButton<String>(
segments: const [ButtonSegment(value: 'Par', label: Text('Par')), ButtonSegment(value: 'Impar', label: Text('Impar'))],
selected: _userChoice != null ? {_userChoice!} : {},
onSelectionChanged: (s) => setState(() => _userChoice = s.first),
),
const SizedBox(height: 24),
const Text('2. Elige un número:', style: TextStyle(fontSize: 16)),
 
Wrap(
spacing: 8.0,
alignment: WrapAlignment.center,
children: List.generate(6, (i) => ElevatedButton(onPressed: () => _play(i), child: Text('$i'))),
),
const SizedBox(height: 32),
OutlinedButton.icon(onPressed: _resetGame, icon: const Icon(Icons.refresh), label: const Text('Reiniciar Marcador')),
],
),
);
}
}
