import {StatusBar} from 'expo-status-bar';
import React, {useState, useEffect} from 'react';
import {Button, StyleSheet, Text, View} from 'react-native';
import {BarCodeScanner} from 'expo-barcode-scanner';
import {backgroundColor} from 'react-native/Libraries/Components/View/ReactNativeStyleAttributes';

export default function App() {
  const [hasPermission, setHasPermission] = useState(null);
  const [scanned, setScanned] = useState(false);
  const [text, setText] = useState('Not yet scanned');

  const askForCameraPermission = async () => {
    const {status} = await BarCodeScanner.requestPermissionsAsync();
    setHasPermission(status === 'granted');
  };

  //Request Camera Permission
  useEffect(() => {
    askForCameraPermission();
  });

  //What happens when a barcode is scanned
  const handleBarCodeScanned = ({type, data}) => {
    setScanned(true);
    setText(data);
    console.log('Type:' + type + '\nData: ' + data);

    //Check permissions and return the screens
    if (hasPermission == null) {
      return (
        <view style={styles.container}>
          <text>Requesting for camera permission</text>
        </view>
      );
    }

    if (hasPermission == false) {
      return (
        <view style={styles.container}>
          <text style={{margin: 10}}> No access to camera</text>
          <Button
            title={'Allow Camera'}
            onPress={() => askForCameraPermission()}>
            {' '}
          </Button>
        </view>
      );
    }
    //Return the view
    return (
      <view style={styles.container}>
        <view style={styles.barcodebox} />
      </view>
    );
  };
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#fff',
    alignItems: 'center',
    justifyContent: 'center',
  },

  barcodebox: {
    backgroundColor: '#fff',
    alightItems: 'center',
    justifyContent: 'center',
    height: '300',
    width: '300',
    overflow: 'hidden',
    borderRadius: '30',
  },
});

