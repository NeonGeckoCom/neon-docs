---
description: >-
  A visual and display framework for Neon running on top of KDE Plasma
  Technology and built using Kirigami, a lightweight user interface framework
  for convergent applications empowered by Qt.
---

# GUI Framework

**Note:** Neon is backwards-compatible with Mycroft code, so despite references here to Mycroft-specific code, it will work with Neon.

In the age of information, visualization is eminently essential to grab attention and create a promising communication strategy. Visual content that supports your spoken content can make it easier to present information well and more engaging for your audience and users.

![](https://images.theconversation.com/files/205921/original/file-20180212-58348-1sbutu2.jpg?ixlib=rb-1.1.0&rect=0%2C604%2C3994%2C1994&q=45&auto=format&w=1356&h=668&fit=crop)

## Introduction

Mycroft-GUI is an open source visual and display framework for Neon running on top of KDE Plasma Technology and built using Kirigami, a lightweight user interface framework for convergent applications which are empowered by Qt.

## Getting Started

Neon is an open source voice assistant that can be extended and expanded to the limits of your imagination. Neon can run anywhere from your desktop to your automobiles or on smart devices that empower your home.

Want Neon to do something new? Teach Neon a skill, share it, and improve the experience for tens of thousands of people all over the world. This guide aims to provide you with resources to create familiar and consistent visual experiences with your expanding and innovative skills.

## Qt Skill GUI

Neon devices with displays, such as the Mark II, provide skill developers the opportunity to create skills that can be empowered by both voice and screen interaction. Skills may provide custom QML resources for customized UIs, or leverage the provided templates for simple interfaces

This section of the guide is divided into two skill examples that will show you how to create:

- In-depth QML based audio and visual interaction skills
- Simple template based text and image skills

### In-depth QML based audio and visual interaction skills

QML user interface markup language is a declarative language built on top of Qt's existing strengths designed to describe the user interface of a program: both what it looks like, and how it behaves. QML provides modules that consist of sophisticated sets of graphical and behavioral building elements. In the example below we will showcase how to create a QML interface for your skill including how it interacts with your voice skill.

#### Before Getting Started: Resources

A collection of resources to familiarize you with QML and Kirigami Framework.

- [Introduction to QML](http://doc.qt.io/qt-5/qml-tutorial.html)
- [Introduction to Kirigami](https://www.kde.org/products/kirigami/)

#### Building your skill to support display

Skills for Mycroft AI are written in Python, using the skills development guide available [here](../../skill_development/)

Let's walk you through some basics of writing your QML user interface, this section is divided into 5 parts:

- [Importing Modules](mycroft-gui.md#importing-modules)
- [Using Mycroft-GUI Framework Base Delegates](mycroft-gui.md#using-mycroft-gui-framework-base-delegates)
- [Using Mycroft Framework Components](mycroft-gui.md#using-mycroft-framework-components)
- [Event Handling](mycroft-gui.md#event-handling)
- [Resting Faces](mycroft-gui.md#resting-faces)

#### Importing Modules

A QML module provides versioned types and JavaScript resources in a type namespace which may be used by clients who import the module. Modules make use of the QML versioning system which allows modules to be independently updated. More in-depth information about QML modules can be found here [Qt QML Modules Documentation](http://doc.qt.io/qt-5/qtqml-modules-topic.html)

In the code snippet example below we will look at importing some of the common modules that provide the components required to get started with our Visual User Interface.

```
import QtQuick 2.4
import QtQuick.Controls 2.2
import QtQuick.Layouts 1.4
import org.kde.kirigami 2.4 as Kirigami
import Mycroft 1.0 as Mycroft
import org.kde.lottie 1.0
```

**QTQuick Module:**

Qt Quick module is the standard library for writing QML applications, the module provides a visual canvas and includes types for creating and animating visual components, receiving user input, creating data models and views and delayed object instantiation. In-depth information about QtQuick can be found at [Qt Quick Documentation](https://doc.qt.io/qt-5.11/qtquick-index.html)

**QTQuick.Controls Module:**

The QtQuick Controls module provides a set of controls that can be used to build complete interfaces in Qt Quick. Some of the controls provided are button controls, container controls, delegate controls, indicator controls, input controls, navigation controls and more. For a complete list of controls and components provided by QtQuick Controls you can refer to [QtQuick Controls 2 Guidelines](https://doc.qt.io/qt-5.11/qtquickcontrols2-guidelines.html)

**QtQuick.Layouts Module:**

QtQuick Layouts are a set of QML types used to arrange items in a user interface. Some of the layouts provided by QtQuick Layouts are Column Layout, Grid Layout, Row Layout and more, for a complete list of layouts you can refer to [QtQuick Layouts Documentation](http://doc.qt.io/qt-5/qtquicklayouts-index.html)

**Kirigami Module:**

[Kirigami](https://api.kde.org/frameworks/kirigami/html/index.html) is a set of QtQuick components for mobile and convergent applications. [Kirigami](https://api.kde.org/frameworks/kirigami/html/index.html) is a set of high level components to make the creation of applications that look and feel great on mobile as well as desktop devices and follow the [Kirigami Human Interface Guidelines](https://community.kde.org/KDE_Visual_Design_Group/KirigamiHIG)

**Mycroft Module:**

Mycroft GUI frameworks provides a set of high level components and events system for aiding in the development of Mycroft visual skills. One of the controls provided by Mycroft GUI frameworks are Mycroft-GUI Framework Base Delegates [Mycroft-GUI Framework Base Delegates Documentation](mycroft-gui.md)

**QML Lottie Module:**

This provides a QML `Item` to render Adobe® After Effects™ animations exported as JSON with Bodymovin using the Lottie Web library. For list of all properties supported refer to [Lottie QML](https://github.com/kbroulik/lottie-qml)

#### Using Mycroft-GUI Framework Base Delegates

When you design your skill with QML, Mycroft-GUI frameworks provides you with some base delegates you should use when designing your GUI skill. The base delegates provide you with a basic presentation layer for your skill with some property assignments that can help you setup background images, background dim, timeout and grace time properties to give you the control you need for rendering an experience. In your GUI Skill you can use:

- Mycroft.Delegate: A basic and simple page based on Kirigami.Page

  Simple display Image and Text Example using Mycroft.Delegate

  ```
  import Mycroft 1.0 as Mycroft

  Mycroft.Delegate {
      skillBackgroundSource: sessionData.exampleImage
      ColumnLayout {
          anchors.fill: parent
          Image {
              id: imageId
              Layout.fillWidth: true
              Layout.preferredHeight: Kirigami.Units.gridUnit * 2
              source: "https://source.unsplash.com/1920x1080/?+autumn"
           }
           Label {
              id: labelId
              Layout.fillWidth: true
              Layout.preferredHeight: Kirigami.Units.gridUnit * 4
              text: "Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua."
          }
      }
  }
  ```

- Mycroft.ScrollableDelegate: A delegate that displays skill visuals in a scroll enabled Kirigami Page.

  Example of using Mycroft.ScrollableDelegate

  ```
  import QtQuick 2.4
  import QtQuick.Controls 2.2
  import QtQuick.Layouts 1.4
  import org.kde.kirigami 2.4 as Kirigami
  import Mycroft 1.0 as Mycroft

  Mycroft.ScrollableDelegate{
      id: root
      skillBackgroundSource: sessionData.background
      property var sampleModel: sessionData.sampleBlob

      Kirigami.CardsListView {
          id: exampleListView
          Layout.fillWidth: true
          Layout.fillHeight: true
          model: sampleModel.lorem
          delegate: Kirigami.AbstractCard {
              id: rootCard
              implicitHeight: delegateItem.implicitHeight + Kirigami.Units.largeSpacing
              contentItem: Item {
                  implicitWidth: parent.implicitWidth
                  implicitHeight: parent.implicitHeight
                  ColumnLayout {
                      id: delegateItem
                      anchors.left: parent.left
                      anchors.right: parent.right
                      anchors.top: parent.top
                      spacing: Kirigami.Units.largeSpacing
                      Kirigami.Heading {
                          id: restaurantNameLabel
                          Layout.fillWidth: true
                          text: modelData.text
                          level: 2
                          wrapMode: Text.WordWrap
                      }
                      Kirigami.Separator {
                          Layout.fillWidth: true
                      }
                      Image {
                          id: placeImage
                          source: modelData.image
                          Layout.fillWidth: true
                          Layout.preferredHeight: Kirigami.Units.gridUnit * 3
                          fillMode: Image.PreserveAspectCrop
                      }
                      Item {
                          Layout.fillWidth: true
                          Layout.preferredHeight: Kirigami.Units.gridUnit * 1
                      }
                  }
              }
          }
      }
  }
  ```

#### Using Mycroft Framework Components

### Simple template based text and image skill displays

Designing a simple skill and only want to display text or images? Mycroft GUI framework and Mycroft enclosure API provide ready to use QML based template wrappers that can minimalisticly display simple skills data such as text and images. In the example below we will showcase how to create a simple voice skill that displays simple text on your Mycroft enabled device with a display.

**Text Example:**

```python
...
def handle_hello_world(self, message):
...
self.gui.show_text("Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmo tempor incididunt ut labore et dolore magna aliqua.")
...
```

**Image Example**

```python
...
def handle_hello_world(self, message):
...
self.gui.show_image("https://source.unsplash.com/1920x1080/?+autumn")
...
```

**HTML URL Example**

```python
...
def handle_hello_world(self, message):
...
self.gui.show_url("https://mycroft.ai")
...
```

**HTML Raw Example**

```
...
def handle_hello_world(self, message):
...
rawhtmlexample = """<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8" />
<title>Simple HTML Example</title>
</head>
<body>
<p>Hello World</p>
</body>
</html>
"""
self.gui.show_html(rawhtmlexample)
...
```

### Advanced skill displays using QML

**Display Lottie Animations**:

You can use the `LottieAnimation` item just like any other `QtQuick` element, such as an `Image` and place it in your scene any way you please.

**QML Example**

```
import QtQuick 2.4
import QtQuick.Controls 2.2
import QtQuick.Layouts 1.4
import org.kde.kirigami 2.4 as Kirigami
import Mycroft 1.0 as Mycroft
import org.kde.lottie 1.0

Mycroft.Delegate {
    LottieAnimation {
        id: fancyAnimation
        anchors.fill: parent
        source: Qt.resolvedUrl("animations/fancy_animation.json")
        loops: Animation.Infinite
        fillMode: Image.PreserveAspectFit
        running: true
    }
}
```

**Display Sliding Images**

Contains an image that will slowly scroll in order to be shown completely

**QML Example**

```
import QtQuick 2.4
import QtQuick.Controls 2.2
import QtQuick.Layouts 1.4
import org.kde.kirigami 2.4 as Kirigami
import Mycroft 1.0 as Mycroft

Mycroft.Delegate {
     background: Mycroft.SlidingImage {
     source: "foo.jpg"
     running: bool    //If true the sliding animation is active
     speed: 1         //Animation speed in Kirigami.Units.gridUnit / second
   }
}
```

**Display Paginated Text**

Takes a long text and breaks it down into pages that can be horizontally swiped

**QML Example**

```
import QtQuick 2.4
import QtQuick.Controls 2.2
import QtQuick.Layouts 1.4
import org.kde.kirigami 2.4 as Kirigami
import Mycroft 1.0 as Mycroft

Mycroft.Delegate {
     Mycroft.PaginatedText {
         text: string      //The text that should be displayed
         currentIndex: 0   //The currently visible page number (starting from 0)
     }
}
```

**Display A Vertical ListView With Information Cards**

Kirigami CardsListView is a ListView which can have AbstractCard as its delegate: it will automatically assign the proper spacing and margins around the cards adhering to the design guidelines.

**Python Skill Example**

```python
...
def handle_food_places(self, message):
...
self.gui["foodPlacesBlob"] = results.json
self.gui.show_page("foodplaces.qml")
...
```

**QML Example**

```
import QtQuick 2.4
import QtQuick.Controls 2.2
import QtQuick.Layouts 1.4
import org.kde.kirigami 2.4 as Kirigami
import Mycroft 1.0 as Mycroft

Mycroft.Delegate{
    id: root
    property var foodPlacesModel: sessionData.foodPlacesBlob

    Kirigami.CardsListView {
        id: restaurantsListView
        Layout.fillWidth: true
        Layout.fillHeight: true
        model: foodPlacesModel
        delegate: Kirigami.AbstractCard {
            id: rootCard
            implicitHeight: delegateItem.implicitHeight + Kirigami.Units.largeSpacing
            contentItem: Item {
                implicitWidth: parent.implicitWidth
                implicitHeight: parent.implicitHeight
                ColumnLayout {
                    id: delegateItem
                    anchors.left: parent.left
                    anchors.right: parent.right
                    anchors.top: parent.top
                    spacing: Kirigami.Units.smallSpacing
                    Kirigami.Heading {
                        id: restaurantNameLabel
                        Layout.fillWidth: true
                        text: modelData.name
                        level: 3
                        wrapMode: Text.WordWrap
                    }
                    Kirigami.Separator {
                        Layout.fillWidth: true
                    }
                    RowLayout {
                        Layout.fillWidth: true
                        Layout.preferredHeight: form.implicitHeight
                        Image {
                            id: placeImage
                            source: modelData.image
                            Layout.fillHeight: true
                            Layout.preferredWidth: placeImage.implicitHeight + Kirigami.Units.gridUnit * 2
                            fillMode: Image.PreserveAspectFit
                        }
                        Kirigami.Separator {
                            Layout.fillHeight: true
                        }
                        Kirigami.FormLayout {
                            id: form
                            Layout.fillWidth: true
                            Layout.minimumWidth: aCard.implicitWidth
                            Layout.alignment: Qt.AlignLeft | Qt.AlignBottom
                            Label {
                                Kirigami.FormData.label: "Description:"
                                Layout.fillWidth: true
                                wrapMode: Text.WordWrap
                                elide: Text.ElideRight
                                text: modelData.restaurantDescription
                            }
                            Label {
                                Kirigami.FormData.label: "Phone:"
                                Layout.fillWidth: true
                                wrapMode: Text.WordWrap
                                elide: Text.ElideRight
                                text: modelData.phone
                            }
                        }
                    }
                }
            }
        }
    }
}
```

**Using Proportional Delegate For Simple Display Skills & Auto Layout**

**ProportionalDelegate** is a delegate which has proportional padding and a columnlayout as mainItem. The delegate supports a proportionalGridUnit which is based upon its size and the contents are supposed to be scaled proportionally to the delegate size either directly or using the proportionalGridUnit.

**AutoFitLabel** is a label that will always scale its text size according to the item size rather than the other way around.

**QML Example**

```
import QtQuick 2.4
import QtQuick.Controls 2.2
import QtQuick.Layouts 1.4
import org.kde.kirigami 2.4 as Kirigami
import Mycroft 1.0 as Mycroft

Mycroft.ProportionalDelegate {
    id: root

    Mycroft.AutoFitLabel {
        id: monthLabel
        font.weight: Font.Bold
        Layout.fillWidth: true
        Layout.preferredHeight: proportionalGridUnit * 40
        text: sessionData.month
    }

    Mycroft.AutoFitLabel {
        id: dayLabel
        font.weight: Font.Bold
        Layout.fillWidth: true
        Layout.preferredHeight: proportionalGridUnit * 40
        text: sessionData.day
    }
}
```

**Using Slideshow Component To Show Cards Slideshow**

Slideshow component lets you insert a slideshow with your custom delegate in any skill display, which can be tuned to autoplay and loop and also scrolled or flicked manually by the user.

**QML Example**

```
import QtQuick 2.4
import QtQuick.Controls 2.2
import QtQuick.Layouts 1.4
import org.kde.kirigami 2.4 as Kirigami
import Mycroft 1.0 as Mycroft

Mycroft.Delegate {
    id: root

    Mycroft.SlideShow {
        id: simpleSlideShow
        model: sessionData.exampleModel // model with slideshow data
        anchors.fill: parent
        interval: 5000 // time to switch between slides
        running: true // can be set to false if one wants to swipe manually
        loop: true // can be set to play through continously or just once
        delegate: Kirigami.AbstractCard {
            width: rootItem.width
            height: rootItem.height
            contentItem: ColumnLayout {
                anchors.fill: parent
                Kirigami.Heading {
                    Layout.fillWidth: true
                    wrapMode: Text.WordWrap
                    level: 3
                    text: modelData.Title
                }
                Kirigami.Separator {
                        Layout.fillWidth: true
                        Layout.preferredHeight: 1
                }
                Image {
                    Layout.fillWidth: true
                    Layout.preferredHeight: rootItem.height / 4
                    source: modelData.Image
                    fillMode: Image.PreserveAspectCrop
                }
            }
        }
    }
}
```

#### Using AudioPlayer Component To Play Audio Files / Audio Streaming

AudioPlayer component is a custom wrapper around Qt Multimedia MediaPlayer, that gives the Skill Authors a basic responsive design audio player they can plug into their skills.

**QML Example**

```
import QtQuick 2.4
import QtQuick.Controls 2.2
import QtQuick.Layouts 1.4
import org.kde.kirigami 2.4 as Kirigami
import Mycroft 1.0 as Mycroft

Mycroft.Delegate {
    id: root
    skillBackgroundSource: sessionData.audioThumbnail

    Mycroft.AudioPlayer {
        id: examplePlayer
        anchors.fill: parent
        source: sessionData.audioSource        //Set URL of audio file
        thumbnail: sessionData.audioThumbnail  //Set Thumbnail of audio
        title: sessionData.audioTitle          //Set Title of audio
        nextAction: "author.example-player.next" //Event to drive next button action in skill
        previousAction: "author.example-player.previous" //Event to drive previous button action in skill
        status: sessionData.status             //Current status of playing audio
    }
}
```

#### Event Handling

Mycroft GUI API provides an Event Handling Protocol between the skill and QML display which allow Skill Authors to forward events in either direction to an event consumer. Skill Authors have the ability to create any amount of custom events. Event names that start with "system." are available to all skills, like previous/next/pick.

**Simple Event Trigger Example From QML Display To Skill**

**Python Skill Example**

```python
    def initialize(self):
    # Initialize...
        self.gui.register_handler('skill.foo.event', self.handle_foo_event)
...
    def handle_foo_event(self, message):
        self.speak(message.data["string"])
...
...
```

**QML Example**

```
import QtQuick 2.4
import QtQuick.Controls 2.2
import QtQuick.Layouts 1.4
import org.kde.kirigami 2.4 as Kirigami
import Mycroft 1.0 as Mycroft

Mycroft.Delegate {
    id: root

    Button {
        anchors.fill: parent
        text: "Click Me"
        onClicked: {
            triggerGuiEvent("skill.foo.event", {"string": "Lorem ipsum dolor sit amet"})
        }
    }
}
```

**Simple Event Trigger Example From Skill To QML Display**

**Python Skill Example**

```python
...
    def handle_foo_intent(self, message):
        self.gui['foobar'] = message.data.get("utterance")
        self.gui['color'] = "blue"
        self.gui.show_page("foo.qml")
...
...
```

**QML Example**

```
import QtQuick 2.4
import QtQuick.Controls 2.2
import QtQuick.Layouts 1.4
import org.kde.kirigami 2.4 as Kirigami
import Mycroft 1.0 as Mycroft

Mycroft.Delegate {
    id: root
    property var fooString: sessionData.foobar

    onFooStringChanged: {
        fooRect.color = sessionData.color
    }

    Rectangle {
        id: fooRect
        anchors.fill: parent
        color: "#fff"
    }
}
```

#### Using VideoPlayer Component To Play Video Files / Video Streaming

VideoPlayer component is a custom wrapper around Qt Multimedia MediaPlayer, that gives the Skill Authors a basic responsive design video player they can plug into their skills.

```
import QtQuick 2.4
import QtQuick.Controls 2.2
import QtQuick.Layouts 1.4
import org.kde.kirigami 2.4 as Kirigami
import Mycroft 1.0 as Mycroft

Mycroft.Delegate {
    id: root
    skillBackgroundSource: sessionData.videoThumbnail

    Mycroft.VidioPlayer {
        id: examplePlayer
        anchors.fill: parent
        source: sessionData.videoSource        //Set URL of video file
        nextAction: "author.example-player.next" //Event to drive next button action in skill
        previousAction: "author.example-player.previous" //Event to drive previous button action in skill
        status: sessionData.status             //Current status of playing video
    }
}
```

#### Resting Faces

The resting face API provides skill authors the ability to extend their skills to supply their own customized IDLE screens that will be displayed when there is no activity on the screen.

**Simple Idle Screen Example**

**Python Skill Example**

```
from mycroft.skills.core import resting_screen_handler
...
@resting_screen_handler('NameOfIdleScreen')
def handle_idle(self, message):
    self.gui.clear()
    self.log.info('Activating foo/bar resting page')
    self.gui["exampleText"] = "This Is A Idle Screen"
    self.gui.show_page('idle.qml')
```

**QML Example**

```
import QtQuick 2.4
import QtQuick.Controls 2.2
import QtQuick.Layouts 1.4
import org.kde.kirigami 2.4 as Kirigami
import Mycroft 1.0 as Mycroft

Mycroft.Delegate {
    id: root
    property var fooString: sessionData.exampleText

    Kirigami.Heading {
        id: headerExample
        anchors.centerIn: parent
        text: fooString
    }
}
```
