<!--
Nextcloud - Tasks

@author Raimund Schlüßler
@copyright 2018 Raimund Schlüßler <raimund.schluessler@mailbox.org>

This library is free software; you can redistribute it and/or
modify it under the terms of the GNU AFFERO GENERAL PUBLIC LICENSE
License as published by the Free Software Foundation; either
version 3 of the License, or any later version.

This library is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU AFFERO GENERAL PUBLIC LICENSE for more details.

You should have received a copy of the GNU Affero General Public
License along with this library.  If not, see <http://www.gnu.org/licenses/>.

-->

<template>
	<draggable element="ol"
		:list="['']"
		:options="{group: 'tasks', swapThreshold: 0.30}"
		:move="onMove"
		@end="onEnd"
	>
		<slot :move="onMove" />
	</draggable>
</template>

<script>
import draggable from 'vuedraggable'

export default {
	components: {
		draggable,
	},
	methods: {
		onMove: function($event) {
			this.cleanUpDragging()
			$event.related.classList.add('dragover')
			$event.stopPropagation()
		},

		onEnd: function($event) {
			// The task to move
			var targetTaskId = $event.item.attributes['task-id']
			if (targetTaskId) {
				targetTaskId = targetTaskId.value
				console.debug('Moved task id: ' + targetTaskId)
			}
			// The new parent task --> make the moved task a subtask
			var taskId = $event.to.attributes['task-id']
			if (taskId) {
				taskId = taskId.value
				console.debug('New parent task id: ' + taskId)
			}
			// The new calendar --> make the moved task a root task
			var calendarId = $event.to.attributes['calendar-id']
			if (calendarId) {
				calendarId = calendarId.value
				console.debug('New calendar id: ' + calendarId)
			}
			// The new collection --> make the moved task a member of this collection
			var collectionId = $event.to.attributes['collection-id']
			if (collectionId) {
				collectionId = collectionId.value
				console.debug('New collection id: ' + collectionId)
			}
			this.cleanUpDragging()
			$event.stopPropagation()
		},

		cleanUpDragging: function() {
			var items = document.getElementsByClassName('task-item')
			for (let i = 0; i < items.length; i++) {
				items[i].classList.remove('dragover')
			}
		},
	}
}
</script>
