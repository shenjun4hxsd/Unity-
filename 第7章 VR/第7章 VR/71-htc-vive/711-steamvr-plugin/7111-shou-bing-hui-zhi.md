##VR手柄的绘制


```csharp
        /*
         * Author : shenjun
         */
        using System.Collections;
        using System.Collections.Generic;
        using UnityEngine;
        using Valve.VR;
    
        /// <summary>
        /// 实现手柄的绘制
        /// </summary>
        [RequireComponent(typeof(SteamVR_RenderModel))]
        public class HandTest : MonoBehaviour {
    
        public enum HandType
        {
            LeftHand,
            RightHand,
        }
    
        public HandType handType = HandType.RightHand;
        uint index;                                     // 设备索引值
    
        SteamVR_Controller.Device controller;           // 控制手柄
    
        SteamVR_Events.Action newPosAction;             // 手柄位置更新
    


        void Awake()
        {
            newPosAction = SteamVR_Events.NewPosesAction(OnNewPos); // 创建位置更新对象
        }
    
        void OnEnable()
        {
            newPosAction.enabled = true;                // 绑定位置更新回调方法
        }
    
        void OnDisable()
        {
            newPosAction.enabled = false;               // 解除位置更新回调方法的绑定
        }
    
    
        IEnumerator Start()
        {
            while (controller == null)
            {
                yield return new WaitForSeconds(1.0f);
    
                var system = OpenVR.System;
                if (system != null)
                {
                    if (handType == HandType.LeftHand)
                    {
                        index = system.GetTrackedDeviceIndexForControllerRole(ETrackedControllerRole.LeftHand);
                    }
                    else 
                    {
                        index = system.GetTrackedDeviceIndexForControllerRole(ETrackedControllerRole.RightHand);
                    }
    
                    if (index == OpenVR.k_unTrackedDeviceIndexInvalid)
                    {
                        continue;
                    }
    
                    SetDeviceIndex((int)index);
                }
            }
    
        }


        /// <summary>
        /// 设置设备的索引
        /// </summary>
        /// <param name="index"></param>
        private void SetDeviceIndex(int index)
        {
            if (controller == null)
            {
                controller = SteamVR_Controller.Input(index);
    
                SteamVR_RenderModel r = GetComponent<SteamVR_RenderModel>();
                if(r)
                {
                    r.SetDeviceIndex(index);
                }
            }
        }
    
        /// <summary>
        /// 设备位置更新
        /// </summary>
        /// <param name="poses"></param>
        private void OnNewPos(TrackedDevicePose_t[] poses)
        {
            if (index == OpenVR.k_unTrackedDeviceIndexInvalid) return;
            if (index >= poses.Length) return;
    
            if(poses[index].bDeviceIsConnected && poses[index].bPoseIsValid)
            {
                var pos = new SteamVR_Utils.RigidTransform(poses[index].mDeviceToAbsoluteTracking);
                transform.localPosition = pos.pos;
                transform.localRotation = pos.rot;
            }
    
        }
}
```