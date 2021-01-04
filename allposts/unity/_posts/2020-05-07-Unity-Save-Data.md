---
layout: post
title: Unity에서의 Data처리
tags: [unity, c#]
comments: true
description: >
 유니티에서 가장 흔하게 사용되는 data 저장 방식에 대해 알아본다.
---

* this unordered seed list will be replaced by toc as unordered list
{:toc}

유니티에서 사용되는 data 저장 방식의 장단점을 알아보고 추천활용법과 예제를 정리하였다.  
맨 아래로 내리면 링크가 나오니 참고할 것.

# PlayerPrefs

- 장점  
Key와 Value 값으로 이루어져있어(Hash, Dictionary와 비슷) 사용하기 편리하다  
유니티에 내장되어있다  
- 단점  
제한적인 자료형만 가능하다(int, float, string) → 하지만 함수로 만들어 가공하여 사용하면 된다  
한 개의 파일에만 저장된다(WebPlayer는 1MB의 용량 제한이 있다)  
보안에 가장 취약하다. 보안처리를 해주어도 값이 노출된다  
레지스트리값으로 저장되기 때문에 기기에서 데이터를 삭제하면 초기화된다  
- 추천 활용법  
값을 변경하더라도 게임 시스템에 크게 지장없는 데이터  
플레이어 설정값 (음악 볼륨값, 사용 언어, 그래픽 등)  
- 암호화 관련 참고 링크  
    [유니티 암호화 1편, PlayerPrefs 암호화](https://www.ikpil.com/1342?category=177563)
- 예제

```csharp
    using System.Collections;
    using System.Collections.Generic;
    using UnityEngine;

    public class PlayerPrefsExample : MonoBehaviour
    {
        private string key = "Example";
        private int val = 0;
        public int defaultValue = 5;

        // 호출 시 갱신
        public int Val
        {
            get
            {
                LoadData();
                return val;
            }
            set
            {
                SaveData(value);
            }
        }

        // 시작 시에 로드
        private void Start()
        {
            PlayerPrefs.DeleteAll();
            if (PlayerPrefs.HasKey(key)) // 키가 있다면 로드
            {
                LoadData();
            }
            else // 키가 없다면 생성
            {
                Debug.Log("┌ Create new PlayerPrefs ┐");
                SaveData(defaultValue);
                Debug.Log("└───────────────┘");
            }
        }

        // 세이브
        private void SaveData(int data)
        {
            PlayerPrefs.SetInt(key, data);
            val = PlayerPrefs.GetInt(key, data);
            Debug.Log("Set: " + val);
        }
        // 로드
        private void LoadData()
        {
            val = PlayerPrefs.GetInt(key, val);
            Debug.Log("Get: " + val);
        }

    }
```

# CSV

- 장점  
엑셀 등의 툴을 사용해 외부에서 작업할 수 있어서 기획과의 협업에 편리하다  
동일하게 대량의 데이터 편집 작업에 용이하다  
- 단점  
표로 만들 수 있는 구조만 가능하기에 Json보다 구조상의 한계가 있다  
특정 키를 찾거나 수정하는 것도 헷갈리고 오류도 발견하기 까다로워서 신뢰성이 떨어진다  
콤마(,)와 엔터(/n)는 따로 큰따옴표("")로 묶어 후처리 해주어야한다  
- 추천 활용법  
외부에서 타인이 수정을 많이 하는 대량의, 정형화된 간단한 구조의 데이터  
로컬라이징 데이터, item table  
- 예제

 ```csharp
    using System.Collections;
    using System.Collections.Generic;
    using UnityEngine;

    /// <summary>
    /// 데이터 가져올떄 파라메터로 사용할 Enum 값
    /// </summary>
    public enum PartDataType
    {
        /// <summary>
        /// 오브젝트 이름
        /// </summary>
        NAME_OBJ,
        /// <summary>
        /// 해당오브젝트 설명
        /// </summary>
        DESCRIPTION
    }

    public class CsvExample : MonoBehaviour
    {

        [SerializeField]
        /// <summary>
        /// 데이터 CSV 파일
        /// </summary>
        TextAsset csvFile;

        // csv를 나누는 기준
        char lineSeperator = '\n';
        char fieldSeperator = ',';

        /// <summary>
        /// 데이터 class
        /// </summary>
        class CsvData
        {
            /// <summary>
            /// 모델오브젝트 이름
            /// </summary>
            public string nameObj;
            /// <summary>
            /// 해당 오브젝트 설명
            /// </summary>
            public string desc;
        }
        /// <summary>
        /// 데이터가 저장될 dictionary
        /// </summary>
        Dictionary<string, CsvData> dataDic = new Dictionary<string, CsvData>();

        private void Start()
        {
            Csv2Dic();
        }

        void Csv2Dic()
        {
            // 행별로 나눠서 저장
            string[] records = csvFile.text.Split(lineSeperator);

            int lineCount = 0;
            int skipLine = 1; // 스킵하고 싶은 라인Count
                              // 열별로 나눠서 저장
            foreach (string record in records)
            {
                lineCount++;

                if (lineCount <= skipLine) // csv에서 라인 스킵
                    continue;

                // value 내 comma 처리 추가
                string[] fields = FieldSplitter(record);

                if (string.IsNullOrEmpty(fields[0]))
                    break;

                // csv 변경시 같이 변경해주어야함-----------
                CsvData objData = new CsvData
                {
                    nameObj = fields[0],
                    desc = fields[1]
                };

                // 정리된 objData를 데이터dic에 최종 Add
                if (dataDic.ContainsKey(fields[0]) == false)
                {
                    dataDic.Add(fields[0], objData);
                    Debug.Log("Added: " + objData.nameObj);
                }
                else
                {
                    Debug.Log("duplicated object name exists");
                }
            }
        }

        /// <summary>
        /// 메인데이터 딕셔너리로부터 string 데이터 가져오기
        /// </summary>
        /// <param name="objName">선택한 오브젝트 이름</param>
        /// <param name="dataName">가져올 데이터종류</param>
        /// <returns></returns>
        public string GetObjData(string objName, PartDataType dataName)
        {
            string data = "";

            if (dataDic.ContainsKey(objName) == false)
            {
                data = "None";
                return data;
            }

            switch (dataName)
            {
                case PartDataType.NAME_OBJ:
                    data = dataDic[objName].nameObj;
                    break;
                case PartDataType.DESCRIPTION:
                    data = dataDic[objName].desc;
                    break;
            }

            return data;
        }

        /// <summary>
        /// value내 comma와 일반 comma의 구분을 위한 ""판별, 반환
        /// </summary>
        /// <param name="line">레코드 입력</param>
        /// <returns></returns>
        internal string[] FieldSplitter(string line)
        {
            List<string> fieldsList = new List<string>();

            int fieldStart = 0;
            bool metDoubleQuotes = false;

            for (int i = 0; i < line.Length; i++)
            {
                if (line[i].Equals('"'))
                {
                    for (i++; line[i].Equals('"') == false; i++) { }
                    metDoubleQuotes = true;
                }

                if (line[i] == fieldSeperator)
                {
                    if (metDoubleQuotes)
                    {
                        fieldsList.Add(line.Substring(fieldStart + 1, i - fieldStart - 2));
                        fieldStart = i + 1;
                        metDoubleQuotes = false;
                    }
                    else
                    {
                        fieldsList.Add(line.Substring(fieldStart, i - fieldStart));
                        fieldStart = i + 1;
                    }
                }

                if (i == line.Length - 1)
                {

                    fieldsList.Add(line.Substring(fieldStart, i - fieldStart + 1));
                }

            }
            return fieldsList.ToArray();
        }
    }
```
- 예제 csv  
```
    skipObject0,Description0
    Object1,Description1
    dupleObject2,Description2
    dupleObject2,Description2-1
    Object3,Description3
    "comma,Object4",Description4
```

# Json

- 장점  
복잡한 구조를 구현할 수 있다  
유니티에서 파서를 지원한다  
Key와 Value 값으로 이루어져있어(Hash, Dictionary와 비슷) 사용하기 편리하다  
- 단점  
CSV와 마찬가지로 오류발견이 까다롭다  
대량의 데이터 편집이 힘들다  
Array 처리를 지원하지 않아 후처리가 필요하다  
- 추천 활용법  
많지 않은, 복잡한 구조의 데이터  
게임 콘텐츠 list  
- 예제

```csharp
    using System.Collections;
    using System.Collections.Generic;
    using UnityEngine;
    using System.IO;
    using System;

    /// <summary>
    /// 데이터 저장과 로드 함수
    /// </summary>
    public class JsonExample : MonoBehaviour
    {
        [Tooltip("저장하길 원하는 파일 이름(.json 제외)")]
        public string fileName = "JsonSample";

        public enum DataType
        {
            Single,
            Array
        }
        [Tooltip("작성한 데이터의 타입 지정")]
        public DataType type = DataType.Single;

        /// <summary>
        /// 받고 싶은 데이터
        /// </summary>
        [System.Serializable]
        public class Data
        {
            public int id;
            public string name;
            public string[] texts;
            public Vector3 position;
        }

        public Data data;
        public Data[] arrayData;

        /// <summary>
        /// 데이터 save 및 load 경로
        /// </summary>
        string DataPath
        {
            get
            { return Path.Combine(Application.dataPath, "Json", fileName + ".json"); }
        }

        // test
        void Start()
        {
            Debug.Log(data.name);
            foreach (var data in arrayData)
            {
                Debug.Log(data.name);
            }
        }

        /// <summary>
        /// 데이터를 Json으로 저장
        /// </summary>
        [ContextMenu("Save Data To Json")]
        void SaveDataToJson()
        {
            string jsonData = "";

            if (type == DataType.Single)
                jsonData = JsonUtility.ToJson(data, true);
            else if (type == DataType.Array)
                jsonData = JsonArrayHelper.ToJson(arrayData, true);

            File.WriteAllText(DataPath, jsonData);
        }

        /// <summary>
        /// Json을 데이터로 불러오기
        /// </summary>
        [ContextMenu("Load Data From Json")]
        void LoadDataFromJson()
        {
            string jsonData = "";
            jsonData = File.ReadAllText(DataPath);

            if (type == DataType.Single)
                data = JsonUtility.FromJson<Data>(jsonData);
            else if (type == DataType.Array)
                arrayData = JsonArrayHelper.FromJson<Data>(jsonData);
        }
    }

    /// <summary>
    /// Json을 Array로 사용하고 싶을 때 JsonUtility대신 사용
    /// </summary>
    public static class JsonArrayHelper
    {
        public static T[] FromJson<T>(string json)
        {
            Wrapper<T> wrapper = JsonUtility.FromJson<Wrapper<T>>(json);
            return wrapper.array;
        }

        public static string ToJson<T>(T[] array, bool prettyPrint = false)
        {
            Wrapper<T> wrapper = new Wrapper<T>();
            wrapper.array = array;
            return JsonUtility.ToJson(wrapper, prettyPrint);
        }

        [System.Serializable]
        private class Wrapper<T>
        {
            public T[] array;
        }
    }
```

# Scriptable Object

- 장점  
유니티에 내장되어있다  
대량의 데이터 처리에 용이하다  
다양하고 복잡한 구조의 데이터도 처리가 가능하다  
- 단점  
빌드 후의 저장이 불가능하다  
유니티 외부에서의 수정이 불가능하다  
- 추천 활용법  
외부에서, 빌드 후 수정할 일이 없는 내부 자체 데이터  
- 예제
- ScriptableObjectExample.cs

```csharp
    using System.Collections;
    using System.Collections.Generic;
    using UnityEngine;

    [CreateAssetMenu]
    public class ScriptableObjectExample : ScriptableObject
    {
        /// <summary>
        /// 받고 싶은 데이터
        /// </summary>
        [System.Serializable]
        public class Data
        {
            public int id;
            public string name;
            public string[] texts;
            public Vector3 position;
        }

        public Data[] datas;
    }
```

- ScriptableObjectTester.cs

```csharp
    using System.Collections;
    using System.Collections.Generic;
    using UnityEngine;

    public class ScriptableObjectTester : MonoBehaviour
    {
        [SerializeField]
        private ScriptableObjectExample example;

        private void Start()
        {
            foreach (var data in example.datas)
            {
                Debug.Log(data.name);
            }
        }
    }
```

# 모든 예제파일
[보러가기](https://github.com/charotiti9/VarietyCodes/tree/master/DataExample)
