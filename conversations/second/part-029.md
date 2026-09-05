# 恢复项目上下文 / 第 029 卷

[本对话目录](README.md) | [总目录](../../README.md) | [Raw](https://raw.githubusercontent.com/weiyang02520-ops/dg202611-conversation-archive/main/conversations/second/part-029.md) | [上一卷](part-028.md) | [下一卷](part-030.md)

## 消息 1457 / user / 分段 1 of 1

~~~~text
weiyang@ubuntu103:~$ sed -n '426,490p' /home/weiyang/dg202611_ws/src/electric-power-inspection-robot/src/ylhb_base/scripts/multisource_fusion_node.py
                self._lidar_geometry_score,
                self._lidar_temporal_match_ratio,
                covariance,
                self._scan_match_score if source == "scan_match" else None,
                self._scan_match_inlier_ratio if source == "scan_match" else None,
                self._scan_match_mean_distance if source == "scan_match" else None,
            )

        def _uwb_measurement(self, now: float) -> UwbPositionMeasurement | None:
            """Construct UWB measurement if available and valid."""
            if not self._uwb_enabled or not self._uwb_alignment_ready:
                return None

            if self._uwb_pose is None or self._uwb_pose_received_at is None:
                return None

            if not self._uwb_quality:
                return None

            # Check freshness
            pose_age = now - self._uwb_pose_received_at
            quality_age = now - self._uwb_quality_received_at if self._uwb_quality_received_at else float('inf')

            if pose_age > self._timeout or quality_age > self._timeout:
                return None

            # Parse quality
            accepted = _bool_value(_value(self._uwb_quality, "accepted", "false"))
            state = _value(self._uwb_quality, "current_state", "REJECTED").upper()

            if not accepted or state not in ["GOOD", "DEGRADED"]:
                return None

            # Transform UWB -> map
            uwb_x = float(self._uwb_pose.pose.pose.position.x)
            uwb_y = float(self._uwb_pose.pose.pose.position.y)

            transformed = self._transform_uwb_to_map(uwb_x, uwb_y)
            if transformed is None:
                return None

            map_x, map_y = transformed

            # Extract metrics
            try:
                residual_rms = float(_value(self._uwb_quality, "residual_rms", "0"))
                geometry_score = float(_value(self._uwb_quality, "geometry_score", "0"))
                unique_anchor_count = int(_value(self._uwb_quality, "unique_anchor_count", "0"))
                confidence = float(_value(self._uwb_quality, "confidence", "0"))
            except (ValueError, TypeError):
                return None

            stamp = _stamp_to_seconds(self._uwb_pose.header.stamp) or now

            return UwbPositionMeasurement(
                x=map_x,
                y=map_y,
                timestamp=stamp,
                state=state,
                fresh=True,
                accepted=accepted,
                residual_rms=residual_rms,
                geometry_score=geometry_score,
                unique_anchor_count=unique_anchor_count,
                confidence=confidence,
weiyang@ubuntu103:~$ 

~~~~

## 消息 1458 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "reasoning_recap",
  "content": "思考了 5s"
}
~~~~

## 消息 1459 / assistant / 分段 1 of 1

~~~~text
这段 wrapper 看起来没明显问题，`accepted/state/confidence` 都确实传进去了。

现在 `UWB_INVALID` 更像是 **fusion core 自己的门限条件没过**，不是 wrapper 没收到数据。

下一步只查它到底为什么判 invalid。执行：

```bash
grep -n -A35 -B15 "UWB_INVALID" \
/home/weiyang/dg202611_ws/src/electric-power-inspection-robot/src/ylhb_base/scripts/multisource_fusion_core.py
```

把输出发我。

我主要看 core 对 `confidence / residual_rms / geometry_score / unique_anchor_count` 的硬门限。你刚才真实 UWB 的 `confidence≈0.13`，很可能就是这里被拒了。
~~~~

## 消息 1460 / user / 分段 1 of 1

~~~~text
weiyang@ubuntu103:~$ grep -n -A35 -B15 "UWB_INVALID" \
/home/weiyang/dg202611_ws/src/electric-power-inspection-robot/src/ylhb_base/scripts/multisource_fusion_core.py
weiyang@ubuntu103:~$ 
~~~~

## 消息 1461 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "reasoning_recap",
  "content": "思考了 4s"
}
~~~~

## 消息 1462 / assistant / 分段 1 of 1

~~~~text
没搜到 `UWB_INVALID` 这个字面量，说明这个原因字符串可能是在别处拼出来的。

下一步只查 core 里真正判断 UWB 可用性的函数：

```bash
grep -n -A45 -B10 "def _uwb_usable" \
/home/weiyang/dg202611_ws/src/electric-power-inspection-robot/src/ylhb_base/scripts/multisource_fusion_core.py
```

把输出发我。
~~~~

## 消息 1463 / user / 分段 1 of 1

~~~~text
weiyang@ubuntu103:~$ sed -n '426,490p' /home/weiyang/dg202611_ws/src/electric-power-inspection-robot/src/ylhb_base/scripts/multisource_fusion_node.py
                self._lidar_geometry_score,
                self._lidar_temporal_match_ratio,
                covariance,
                self._scan_match_score if source == "scan_match" else None,
                self._scan_match_inlier_ratio if source == "scan_match" else None,
                self._scan_match_mean_distance if source == "scan_match" else None,
            )

        def _uwb_measurement(self, now: float) -> UwbPositionMeasurement | None:
            """Construct UWB measurement if available and valid."""
            if not self._uwb_enabled or not self._uwb_alignment_ready:
                return None

            if self._uwb_pose is None or self._uwb_pose_received_at is None:
                return None

            if not self._uwb_quality:
                return None

            # Check freshness
            pose_age = now - self._uwb_pose_received_at
            quality_age = now - self._uwb_quality_received_at if self._uwb_quality_received_at else float('inf')

            if pose_age > self._timeout or quality_age > self._timeout:
                return None

            # Parse quality
            accepted = _bool_value(_value(self._uwb_quality, "accepted", "false"))
            state = _value(self._uwb_quality, "current_state", "REJECTED").upper()

            if not accepted or state not in ["GOOD", "DEGRADED"]:
                return None

            # Transform UWB -> map
            uwb_x = float(self._uwb_pose.pose.pose.position.x)
            uwb_y = float(self._uwb_pose.pose.pose.position.y)

            transformed = self._transform_uwb_to_map(uwb_x, uwb_y)
            if transformed is None:
                return None

            map_x, map_y = transformed

            # Extract metrics
            try:
                residual_rms = float(_value(self._uwb_quality, "residual_rms", "0"))
                geometry_score = float(_value(self._uwb_quality, "geometry_score", "0"))
                unique_anchor_count = int(_value(self._uwb_quality, "unique_anchor_count", "0"))
                confidence = float(_value(self._uwb_quality, "confidence", "0"))
            except (ValueError, TypeError):
                return None

            stamp = _stamp_to_seconds(self._uwb_pose.header.stamp) or now

            return UwbPositionMeasurement(
                x=map_x,
                y=map_y,
                timestamp=stamp,
                state=state,
                fresh=True,
                accepted=accepted,
                residual_rms=residual_rms,
                geometry_score=geometry_score,
                unique_anchor_count=unique_anchor_count,
                confidence=confidence,
weiyang@ubuntu103:~$ grep -n -A35 -B15 "UWB_INVALID" \
/home/weiyang/dg202611_ws/src/electric-power-inspection-robot/src/ylhb_base/scripts/multisource_fusion_core.py
weiyang@ubuntu103:~$ grep -n -A45 -B10 "def _uwb_usable" \
/home/weiyang/dg202611_ws/src/electric-power-inspection-robot/src/ylhb_base/scripts/multisource_fusion_core.py
647-        enu = geodetic_to_enu(
648-            GeodeticPoint(gnss.latitude, gnss.longitude, gnss.altitude),
649-            self.config.gnss_reference,  # type: ignore[arg-type]
650-        )
651-        x, y = self._alignment.apply(enu)  # type: ignore[union-attr]
652-        return (
653-            Pose2D(x, y, self._map_pose.yaw if self._map_pose is not None else 0.0, gnss.timestamp),
654-            compute_gnss_confidence(gnss, self.config),
655-        )
656-
657:    def _uwb_usable(self, uwb: UwbPositionMeasurement, now: float, reasons: list[str]) -> bool:
658-        """Check if UWB measurement is usable."""
659-        if not uwb.accepted:
660-            reasons.append("UWB_REJECTED")
661-            return False
662-        if not uwb.fresh or uwb.timestamp > now + 1e-6 or now - uwb.timestamp > self.config.freshness_timeout:
663-            reasons.append("UWB_STALE")
664-            return False
665-        if uwb.state.upper() == REJECTED:
666-            reasons.append("UWB_REJECTED")
667-            return False
668-        if uwb.state.upper() == STALE:
669-            reasons.append("UWB_STALE")
670-            return False
671-        return all(_finite(value) for value in (uwb.x, uwb.y, uwb.timestamp))
672-
673-    def _uwb_update(self, uwb: UwbPositionMeasurement | None, now: float, reasons: list[str]) -> tuple[Pose2D, float] | None:
674-        """Process UWB measurement and return (pose, confidence) or None."""
675-        if uwb is None or not self._uwb_usable(uwb, now, reasons):
676-            return None
677-        # UWB provides x/y in map frame; keep current yaw estimate
678-        return (
679-            Pose2D(uwb.x, uwb.y, self._map_pose.yaw if self._map_pose is not None else 0.0, uwb.timestamp),
680-            compute_uwb_confidence(uwb, self.config),
681-        )
682-
683-    def _select_lidar_update(
684-        self, event: FusionInput, now: float, reasons: list[str]
685-    ) -> tuple[MapPoseMeasurement | None, str | None]:
686-        scan = event.scan_match_pose
687-        if scan is not None and self._measurement_usable(scan, now):
688-            return scan, "SCAN_MATCH"
689-        if scan is not None and scan.accepted and scan.state.upper() not in {REJECTED, STALE}:
690-            reasons.append("LIDAR_SCAN_MATCH_NOT_USABLE")
691-        amcl = event.amcl_pose
692-        if amcl is not None and self._measurement_usable(amcl, now):
693-            return amcl, "AMCL"
694-        if amcl is not None and amcl.state.upper() not in {REJECTED, STALE}:
695-            reasons.append("LIDAR_AMCL_NOT_USABLE")
696-        return None, None
697-
698-    def _measurement_usable(self, measurement: MapPoseMeasurement, now: float) -> bool:
699-        return (
700-            measurement.accepted
701-            and measurement.fresh
702-            and measurement.state.upper() == GOOD
weiyang@ubuntu103:~$ 

~~~~

## 消息 1464 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "reasoning_recap",
  "content": "思考了 6s"
}
~~~~

## 消息 1465 / assistant / 分段 1 of 1

~~~~text
找到了一个关键点：`_uwb_usable()` 本身不会产生 `UWB_INVALID`，所以这个字符串大概率是在 **ROS2 wrapper 的 `_tick()` 里，当 `_uwb_measurement()` 返回 `None` 时补进去的**。

现在只查这一处：

```bash
grep -n -A25 -B15 "UWB_INVALID" \
/home/weiyang/dg202611_ws/src/electric-power-inspection-robot/src/ylhb_base/scripts/multisource_fusion_node.py
```

把输出发我。

这样我们就能直接知道到底是：
- wrapper 根本没构造出 UWB measurement，
还是
- 构造出来了但 core 拒绝了。

目前我更怀疑前者。
~~~~

## 消息 1466 / user / 分段 1 of 1

~~~~text
weiyang@ubuntu103:~$ 
weiyang@ubuntu103:~$ grep -n -A25 -B15 "UWB_INVALID" \
/home/weiyang/dg202611_ws/src/electric-power-inspection-robot/src/ylhb_base/scripts/multisource_fusion_node.py
weiyang@ubuntu103:~$ grep -n -A25 -B15 "UWB_INVALID" /home/weiyang/dg202611_ws/src/electric-power-inspection-robot/src/ylhb_base/scripts/multisource_fusion_node.py
weiyang@ubuntu103:~$ 
~~~~

## 消息 1467 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "reasoning_recap",
  "content": "思考了 7s"
}
~~~~

## 消息 1468 / assistant / 分段 1 of 1

~~~~text
明白了，`UWB_INVALID` 不是硬编码字符串，应该是动态拼出来的。

现在只查所有 `INVALID` 相关逻辑：

```bash
grep -n -E "INVALID|uwb_measurement|reasons.append" \
/home/weiyang/dg202611_ws/src/electric-power-inspection-robot/src/ylhb_base/scripts/multisource_fusion_node.py \
/home/weiyang/dg202611_ws/src/electric-power-inspection-robot/src/ylhb_base/scripts/multisource_fusion_core.py
```

把输出发我。这样基本就能直接定位 `UWB_INVALID` 是哪一层产生的。
~~~~

## 消息 1469 / user / 分段 1 of 1

~~~~text
weiyang@ubuntu103:~$ 
weiyang@ubuntu103:~$ grep -n -A25 -B15 "UWB_INVALID" \
/home/weiyang/dg202611_ws/src/electric-power-inspection-robot/src/ylhb_base/scripts/multisource_fusion_node.py
weiyang@ubuntu103:~$ grep -n -A25 -B15 "UWB_INVALID" /home/weiyang/dg202611_ws/src/electric-power-inspection-robot/src/ylhb_base/scripts/multisource_fusion_node.py
weiyang@ubuntu103:~$ grep -n -E "INVALID|uwb_measurement|reasons.append" \
/home/weiyang/dg202611_ws/src/electric-power-inspection-robot/src/ylhb_base/scripts/multisource_fusion_node.py \
/home/weiyang/dg202611_ws/src/electric-power-inspection-robot/src/ylhb_base/scripts/multisource_fusion_core.py
/home/weiyang/dg202611_ws/src/electric-power-inspection-robot/src/ylhb_base/scripts/multisource_fusion_node.py:434:        def _uwb_measurement(self, now: float) -> UwbPositionMeasurement | None:
/home/weiyang/dg202611_ws/src/electric-power-inspection-robot/src/ylhb_base/scripts/multisource_fusion_node.py:499:            new_uwb = self._uwb_measurement(now) if self._uwb_sequence != self._uwb_consumed_sequence else None
/home/weiyang/dg202611_ws/src/electric-power-inspection-robot/src/ylhb_base/scripts/multisource_fusion_core.py:461:            return self._output(event.now, INITIALIZING, None, False, ("INVALID_NOW",))
/home/weiyang/dg202611_ws/src/electric-power-inspection-robot/src/ylhb_base/scripts/multisource_fusion_core.py:465:                reasons.append("FUSION_TIME_REVERSED")
/home/weiyang/dg202611_ws/src/electric-power-inspection-robot/src/ylhb_base/scripts/multisource_fusion_core.py:474:            reasons.append("LOCAL_ODOM_REJECTED")
/home/weiyang/dg202611_ws/src/electric-power-inspection-robot/src/ylhb_base/scripts/multisource_fusion_core.py:478:                reasons.append("GNSS_ALIGNMENT_UNAVAILABLE" if self._alignment is None else "GNSS_NOT_USABLE")
/home/weiyang/dg202611_ws/src/electric-power-inspection-robot/src/ylhb_base/scripts/multisource_fusion_core.py:609:            reasons.append("LOCAL_ODOM_INVALID")
/home/weiyang/dg202611_ws/src/electric-power-inspection-robot/src/ylhb_base/scripts/multisource_fusion_core.py:614:            reasons.append("LOCAL_ODOM_TIME_REVERSED")
/home/weiyang/dg202611_ws/src/electric-power-inspection-robot/src/ylhb_base/scripts/multisource_fusion_core.py:629:            reasons.append("GNSS_REJECTED")
/home/weiyang/dg202611_ws/src/electric-power-inspection-robot/src/ylhb_base/scripts/multisource_fusion_core.py:632:            reasons.append("GNSS_STALE")
/home/weiyang/dg202611_ws/src/electric-power-inspection-robot/src/ylhb_base/scripts/multisource_fusion_core.py:635:            reasons.append("GNSS_REJECTED")
/home/weiyang/dg202611_ws/src/electric-power-inspection-robot/src/ylhb_base/scripts/multisource_fusion_core.py:638:            reasons.append("GNSS_STALE")
/home/weiyang/dg202611_ws/src/electric-power-inspection-robot/src/ylhb_base/scripts/multisource_fusion_core.py:660:            reasons.append("UWB_REJECTED")
/home/weiyang/dg202611_ws/src/electric-power-inspection-robot/src/ylhb_base/scripts/multisource_fusion_core.py:663:            reasons.append("UWB_STALE")
/home/weiyang/dg202611_ws/src/electric-power-inspection-robot/src/ylhb_base/scripts/multisource_fusion_core.py:666:            reasons.append("UWB_REJECTED")
/home/weiyang/dg202611_ws/src/electric-power-inspection-robot/src/ylhb_base/scripts/multisource_fusion_core.py:669:            reasons.append("UWB_STALE")
/home/weiyang/dg202611_ws/src/electric-power-inspection-robot/src/ylhb_base/scripts/multisource_fusion_core.py:690:            reasons.append("LIDAR_SCAN_MATCH_NOT_USABLE")
/home/weiyang/dg202611_ws/src/electric-power-inspection-robot/src/ylhb_base/scripts/multisource_fusion_core.py:695:            reasons.append("LIDAR_AMCL_NOT_USABLE")
/home/weiyang/dg202611_ws/src/electric-power-inspection-robot/src/ylhb_base/scripts/multisource_fusion_core.py:719:            reasons.append(f"{source}_INVALID")
/home/weiyang/dg202611_ws/src/electric-power-inspection-robot/src/ylhb_base/scripts/multisource_fusion_core.py:725:            reasons.append(f"{source}_OLD_MEASUREMENT")
/home/weiyang/dg202611_ws/src/electric-power-inspection-robot/src/ylhb_base/scripts/multisource_fusion_core.py:731:            reasons.append(f"{source}_OUTLIER")
weiyang@ubuntu103:~$ 
~~~~

## 消息 1470 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "model_editable_context",
  "model_set_context": ""
}
~~~~

## 消息 1471 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "reasoning_recap",
  "content": "思考了 4s"
}
~~~~

[本对话目录](README.md) | [总目录](../../README.md) | [Raw](https://raw.githubusercontent.com/weiyang02520-ops/dg202611-conversation-archive/main/conversations/second/part-029.md) | [上一卷](part-028.md) | [下一卷](part-030.md)
