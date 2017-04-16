##NavMeshAgent 导航网格代理

**描述：**

导航网格代理。该组件附加于游戏中移动角色上，允许使用导航网格在场景中进行导航。


|变量|说明|
|:--|:--|
|acceleration|该代理跟随路径的最大加速度，指定单位/秒^2。|
|angularSpeed|跟随路径的最大转向速度单位(度/秒)。|
|areaMask|指定导航网格区域是可行的。该变区域遮挡将会使路径变旧(请参考isPathStale)。|
|autoBraking|该代理是否自动制动去避免超过目的点？|
|autoRepath|当现在的路径变成无效时该代理是否尝试获取新路径？|
|autoTraverseOffMeshLink|该代理是否自动穿过OffMeshLinks？|
|avoidancePriority|逃避优先级。|
|baseOffset|对象自身的相对垂直位移。|
|**currentOffMeshLinkData**|当前分离网格连接数据。|
|desiredVelocity|该代理的需求速度包括任何逃避时潜在的贡献(只读)。|
|destination|获取或者尝试去在世界空间单位中设置代理的目的地。|
|hasPath|当前代理是否有路径(只读)？|
|height|代理经过障碍物下方目的高度，等等。|
|isOnNavMesh|当前代理束缚到导航网格？(只读)|
|**isOnOffMeshLink**|当前代理位置是否在OffMeshLink上?(只读)|
|isPathStale|当前路径是否是旧路径。(只读)|
|nextOffMeshLinkData|在当前路径上的下一个OffMeshLinkData。|
|nextPosition|获取或者设置导航网格的模拟仿真位置。|
|obstacleAvoidanceType|逃避的质量的级别。|
|path|获取或者设置当前路径属性。|
|pathPending|该路径在被计算的过程中但是还没准备？(只读)|
|pathStatus|当前路况的状态(完整，局部或者无效)。|
|radius|代理的逃避半径。|
|**remainingDistance**|在当前路径上代理的位置与目的地之间的距离。(只读)|
|speed|当跟随路径时的最大移动速度。|
|steeringTarget|获取当前沿着路径转向的目标(只读)。|
|stoppingDistance|从目标位置开始，在该距离之内刹车。|
|updatePosition|获取或者设置变换位置与模拟代理位置同步。默认值为true。|
|updateRotation|代理是否应该更新方向？|
|velocity|使用NavMeshAgent 组件的当前速度，或者手动设置速度去控制代理。|

---

|方法|说明|
|:--|:--|
|ActivateCurrentOffMeshLink|启用或者禁用当前分离网格链接。|
|**CalculatePath**|	计算出路径指定点并存储结果路径。|
|**CompleteOffMeshLink**|在当前分离网格链接上完成运动。|
|FindClosestEdge|查找导航网格最近的边缘。|
|**GetAreaCost**|当穿过特别类型的区域时获取路径计算成本。|
|Move|应用相对运动到当前位置。|
|Raycast|在导航网格中朝着目标方向追踪直线路径而不移动代理。|
|ResetPath|清除当前路径。|
|**Resume**|暂停之后沿着当前路径重新恢复运动。|
|SamplePathPosition|沿着当前路径的样本位置。|
|**SetAreaCost**|设置穿越区域的区域类型的成本。|
|**SetDestination**|设置或者更新目的地因此触发计算新的路径。|
|SetPath|分配新路径给该代理。|
|Stop|该代理沿着它的当前路径的刹车运动。|
|Warp|弯曲代理提供位置。|

---


```csharp
        NavMeshPath path = new NavMeshPath ();
		bool havePath = m_pNavAgent.CalculatePath (m_pTarget.position, path);

        float fGrassCost = m_pNavAgent.GetLayerCost (4);
		if (fGrassCost == 3) {

		}

		m_pNavAgent.SetLayerCost(3, 10);
```
```csharp
        if (m_pNavAgent.isOnOffMeshLink) { // 判断是否到达起点
			m_fTimeCount += Time.deltaTime;

			Vector3 currtPos = Vector3.Lerp(m_pNavAgent.currentOffMeshLinkData.startPos, m_pNavAgent.currentOffMeshLinkData.endPos, m_fTimeCount);
			currtPos.y += Mathf.Sin(Mathf.Clamp01(m_fTimeCount) * Mathf.PI)*4;

			transform.position = currtPos;

			if(Vector3.Distance(transform.position, m_pNavAgent.currentOffMeshLinkData.endPos) < 0.5f)
			{
				m_pNavAgent.CompleteOffMeshLink(); // 完成移动后结束移动过程。
			}
		}
```



🔚
