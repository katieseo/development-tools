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

#### 4. components: View, ScrollView, Text, Button, TextInput, 
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

6. APIs



