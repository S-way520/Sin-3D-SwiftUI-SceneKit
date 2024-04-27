# Sin-3D-SwiftUI-SceneKit
# The first part
![IMG_2058](https://github.com/S-way520/Sin-3D-SwiftUI-SceneKit/assets/95877651/10201620-ecb9-452a-82f7-ca4661c6bbd9)

# The second part
Code file 1
```swift
import SwiftUI
@main
struct MyApp: App {
    var body: some Scene {
        WindowGroup {
            ContentView()
        }
    }
}
```
Code flie 2
```swift
import SceneKit
import SwiftUI

struct ContentView: View {
    var body: some View {
        Sine3D()
    }
}

struct Sine3D: View {
    @State var Amplitude: Float = 3
    @State var frequency: Float = 1
    var body: some View {
        ZStack {
            ZStack {
                hb121Sine3D(
                    A: $Amplitude, 
                    F: $frequency
                )
            }
            HStack {
                Spacer()
                VStack {
                    Stepper(value: $Amplitude, in: 1...4) {
                        Text("Amplitude：\(Int(Amplitude))")
                            .foregroundColor(.green)
                    }
                    Stepper(value: $frequency, in: 1...8) {
                        Text("Frequency：\(Int(frequency))")
                            .foregroundColor(.red)
                    }
                    Spacer()
                }
                .frame(width: 250)
            }
            .padding()
        }
    }
}
struct hb121Sine3D: View {
    @Binding var A: Float
    @Binding var F: Float
    let Node = SCNNode()
    var sine: SCNNode {
        var vertices: [SCNVector3] = []
        for i in 0...6250 {
            let x = Float(i) / 1000
            let y = A * sin(F * x)
            let z = A * cos(F * x)
            let vertex = SCNVector3(x, y, z)
            vertices.append(vertex)
        }
        let sphereGeometry = SCNSphere(radius: 0.05)
        sphereGeometry.firstMaterial?.diffuse.contents = UIColor.cyan
        let sphereNode = SCNNode(geometry: sphereGeometry)
        for vertex in vertices {
            let pointNode = sphereNode.clone()
            pointNode.position = vertex
            Node.addChildNode(pointNode)
        }
        return Node
    }
    
    var camera: SCNNode {
        let camera = SCNCamera()
        let cameraNode = SCNNode()
        cameraNode.position = .init(3, 0, 20)
        cameraNode.camera = camera
        return cameraNode
    }
    
    var scene: SCNScene {
        let scene = SCNScene()
        scene.background.contents = UIColor.systemBackground
        scene.rootNode.addChildNode(sine)
        scene.rootNode.addChildNode(camera)
        return scene
    }
    
    var body: some View {
        SceneView(
            scene: scene,
            options: [.autoenablesDefaultLighting, .allowsCameraControl]
        )
    }
    
}

```
