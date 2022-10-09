## Setting up

#### 1. create expo
https://reactnative.dev/docs/environment-setup

```
npx create-expo-app AwesomeProject
cd AwesomeProject
```

#### 2. Download Expo on the phone

#### 3. npm start 
    a. QR Code to run on the phone

    b. simulator on the computer
      - Xcode for iOS (only on the mac): 
        1)open xcode > go to reference > Locations > Command Line Tools: Xcode 13.2.1.(select Xcode)
        2)Applications - Xcode right click > Show Package Contents ( Contents / Developer / Applications / Simulator.app < double click )
        3)press 'i' on the terminal
        
      - Android Studio: more actions click - Virtual Device Manager > Play Button (press 'a' on the terminal)

#### 4. components: View, Text, Button, TextInput / ScrollView, FlatList, Item / 
####     Pressable, Modal
```
<Button
  onPress={onPressLearnMore}
  title="Learn More"
  color="#841584"
  accessibilityLabel="Learn more about this purple button"
/>

<Image source={require('@expo/snack-static/react-native-logo.png')} />
```
```
const UselessTextInput = () => {
  const [text, onChangeText] = React.useState("Useless Text");
  const [number, onChangeNumber] = React.useState(null);

  return (
    <SafeAreaView>
      <TextInput
        style={styles.input}
        onChangeText={onChangeText}
        value={text}
      />
      <TextInput
        style={styles.input}
        onChangeText={onChangeNumber}
        value={number}
        placeholder="useless placeholder"
        keyboardType="numeric"
      />
    </SafeAreaView>
  );
};
```
```
<View style={styles.todoList}>. // set height (flex: 3)
    <ScrollView>        
    </ScrollView>
</View>
```
```
FlatList: lazyloading

const DATA = [
  {
    id: 'bd7acbea-c1b1-46c2-aed5-3ad53abb28ba',
    title: 'First Item',
  },
  {
    id: '3ac68afc-c605-48d3-a4f8-fbd91aa97f63',
    title: 'Second Item',
  },
  {
    id: '58694a0f-3da1-471f-bd96-145571e29d72',
    title: 'Third Item',
  },
];

const Item = ({ title }) => (
  <View style={styles.item}>
    <Text style={styles.title}>{title}</Text>
  </View>
);

const App = () => {
  const renderItem = ({ item }) => (
    <Item title={item.title} />
  );

  return (
    <SafeAreaView style={styles.container}>
      <FlatList
        data={DATA}
        renderItem={renderItem}
        keyExtractor={item => item.id}
      />
    </SafeAreaView>
  );
}
```
```
<Pressable onPress={onPressFunction}>
  <Text>I'm pressable!</Text>
</Pressable>
```
```
const App = () => {
  const [modalVisible, setModalVisible] = useState(false);
  return (
    <View style={styles.centeredView}>
      <Modal
        animationType="slide"
        transparent={true}
        visible={modalVisible}
        onRequestClose={() => {
          Alert.alert("Modal has been closed.");
          setModalVisible(!modalVisible);
        }}
      >
        <View style={styles.centeredView}>
          <View style={styles.modalView}>
            <Text style={styles.modalText}>Hello World!</Text>
            <Pressable
              style={[styles.button, styles.buttonClose]}
              onPress={() => setModalVisible(!modalVisible)}
            >
              <Text style={styles.textStyle}>Hide Modal</Text>
            </Pressable>
          </View>
        </View>
      </Modal>
      <Pressable
        style={[styles.button, styles.buttonOpen]}
        onPress={() => setModalVisible(true)}
      >
        <Text style={styles.textStyle}>Show Modal</Text>
      </Pressable>
    </View>
  );
};

const styles = StyleSheet.create({
  centeredView: {
    flex: 1,
    justifyContent: "center",
    alignItems: "center",
    marginTop: 22
  },
  modalView: {
    margin: 20,
    backgroundColor: "white",
    borderRadius: 20,
    padding: 35,
    alignItems: "center",
    shadowColor: "#000",
    shadowOffset: {
      width: 0,
      height: 2
    },
    shadowOpacity: 0.25,
    shadowRadius: 4,
    elevation: 5
  },
  button: {
    borderRadius: 20,
    padding: 10,
    elevation: 2
  },
  buttonOpen: {
    backgroundColor: "#F194FF",
  },
  buttonClose: {
    backgroundColor: "#2196F3",
  },
  textStyle: {
    color: "white",
    fontWeight: "bold",
    textAlign: "center"
  },
  modalText: {
    marginBottom: 15,
    textAlign: "center"
  }
});

export default App;
```

#### 5. Styles

* borderRadius is not working on Text component on iOS > should wrap in View and add radius on View
* style color on the component, parent color wouldn't change the child. styles don't cascade

```
container: { flex: 1 },   //takeup all height
```
```
<Text style={{margin: 20, borderWidth: 2, borderColor: 'red', paddingHorizontal: 10, padding}}>Hello</Text>
```
```
<View style={styles.container}></View>

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: "#fff",
    alignItems: "center",
    justifyContent: "center",
  }
});

```

#### 6. Interaction

onChangeText, onSubmitEditing, onFocus

```
<Text onChangeText={inputHandler}>Text</Text>

function inputHandler(enteredText) {
    console.log(enteredText)
}
```
android_ripple, style press(iOS) - click effect
```
<Pressable
    android_ripple = {{ color: '#ddd' }}
    style={(pressData) => pressData.pressed && styles.pressedItem}
>
    {({ pressed }) => <Text> {pressed ? "X Pressed" : "X"} </Text>}
</Pressable>

styles...
    pressedItem: {
        opacity: 0.5
    }

```
6. APIs



